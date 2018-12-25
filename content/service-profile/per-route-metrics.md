---
date: 2018-12-25T14:20:00+08:00
title: Service Profiles for Per-Route Metrics
weight: 1010
menu:
  main:
    parent: "service-profile"
description : "Service Profiles for Per-Route Metrics"
---

> 内容摘要自： [Service Profiles for Per-Route Metrics](https://blog.linkerd.io/2018/12/07/service-profiles-for-per-route-metrics/)，这里介绍了service profile在每route的metrics中的使用。

Linkerd的主要目标之一是使服务和平台所有者能够理解他们的服务：不仅在数据包和字节方面，而且在请求和响应方面。对于HTTP等协议，这需要了解协议的语义，以便Linkerd可以测量成功率和响应延迟等内容。我们还需要能够在不同维度（例如源服务，目标服务和HTTP路径）中分解这些指标。

但是，通过路径聚合指标尤其会带来一些重大挑战。在这篇文章中，我们将探讨这些挑战，并展示Linkerd如何使用称为service profile的新概念来处理它们。

### 第一次尝试

将成为Linkerd 2.0的最早版本（当它被称为Conduit时）实际上具有开箱即用的每路径指标：

![](https://blog.linkerd.io/wp-content/uploads/sites/3/2018/12/image-1.png)

这个功能非常有用，但是有一个很大的问题。Conduit 将所有指标存储在Prometheus中，使用Prometheus lable 存储数据的所有不同维度，例如service，deployment和path。Path的Prometheus label 意味着每条独特的 Path 都会在 Prometheus 中创建新的时间序列。由于Conduit无法控制它收到的请求，因此Prometheus很容易被无限数量的唯一Path所导致的无数的时间序列所淹没。事实上，Prometheus的文档包括对这个问题的警告！虽然这个早期版本向我们展示了这个功能有多棒，但我们最终还是如此决定删除Path label，以确保在Linkerd中，Prometheus可以继续表现良好。

### Linkerd Top

“别急！”你可能会说，“linkerd top命令呢？这不是显示每个路径的指标吗？”你是对的，确实如此！

![](https://blog.linkerd.io/wp-content/uploads/sites/3/2018/12/image-2.png)

linkerd top命令通过完全避开Prometheus来回避时间序列的基数问题。它直接使用实时流量流（使用和linkerd tap相同的机制）并在内存中进行每路径聚合。这有一些非常强大的优势：

- 由于结果是直接从实时流量计算的，因此数据是真实的。没有等待指标被抓取和聚合的延迟。
- 没有让Prometheus爆炸的担忧。

当然，这种方法也有一些缺点：

- 没有时间序列意味着没有历史数据。仅限实时数据。
- 在高流量速率下，可能对实时业务流进行采样。这意味着，数据的在linkerd top被不能保证代表请求的100％，并且不应该被认为是100％精确的。

尽管如此，linkerd top仍然是一个非常有用的工具，可以快速了解实时流量的表现。

### 烦人的路径参数问题

对于每路径的metrics，还有一个主要问题，不管是基于普罗米修斯还是基于实时流量的方法。看看你能否在这个截图中找到它：

![](https://blog.linkerd.io/wp-content/uploads/sites/3/2018/12/image-3.png)

当路径中包含参数（如用户名或ID）时，通常没有必要单独计算每个路径的度量。通常需要的是为一组相似路径聚合在一起的指标。在上面的截图中，我们很想看到的是 `/books/*` 的指标。在Linkerd中，我们将一组路径称为 route。

### service profile

Linkerd 2.1引入了service profile的概念。service profile是自定义Kubernetes资源，它部署在Linkerd控制平面命名空间中，并允许运维为Linkerd提供有关服务的其他信息。特别是，它允许您定义服务的route列表。每个route使用正则表达式来定义哪些路径应与该route匹配。我们来看一个定义2个route的service profile示例。

```yaml
apiVersion: linkerd.io/v1alpha1
kind: ServiceProfile
metadata:
  name: webapp.default.svc.cluster.local
  namespace: linkerd
spec:
  routes:
  - name: '/books' # This is used as the value for the rt_route label
    condition:
      method: POST
      pathRegex: '/books'
  - name: '/books/{id}' # This is used as the value for the rt_route label
    condition:
      method: GET
      pathRegex: '/books/\d+'
```

Service Profile中的每个route都包含name和condition。Condition与Method匹配，并使用正则表达式匹配Path。匹配route的请求将 rt_routePrometheus 标签设置为route名称。

通过要求在service profile中手动定义route，Linkerd能够解决以前方法的许多问题：

- Path 以用户定义的方式聚合，可以匹配应用程序的语义。
- Route 必须显式配置，因此route的数量（和时间序列的数量）是有限的。
- 不需要对route度量进行采样，并且在度量的计算中计算每个请求。
- Route度量在Prometheus 中存储为时间序列数据，因此可以在事后查询历史数据。

让我们来看一个端到端的例子。

### 每Route Metrics示例

以下是一个快速示例，您可以在家中尝试使用Linkerd获取每个路由指标是多么容易。首先将Linkerd和我们的示例Books应用程序安装到您的Kubernetes集群中。

```bash
linkerd install | kubectl apply -f -
linkerd check
curl https://run.linkerd.io/booksapp.yml | linkerd inject - | kubectl apply -f -
```
 
此时，Books应用程序已安装并从内置流量生成器接收流量。我们希望看到 `webapp` 服务 每route metrics -  但我们不能，因为我们还没有定义该服务的任何route呢！

```bash
$ linkerd routes svc/webapp
ROUTE       SERVICE   SUCCESS      RPS   LATENCY_P50   LATENCY_P95   LATENCY_P99
[UNKNOWN]    webapp    70.00%   5.7rps          34ms         100ms         269ms
```
 
我们可以看到 webapp 服务有流量但除此之外看不到其他。让我们通过使用 linkerd profile 命令创建service profile来解决这个问题。

```bash
$ linkerd profile --template webapp > webapp-profile.yaml
```

`linkerd profile --template` 命令生成基本service profile规范，您可以编辑该规范以定义所需的路由。让我们编辑webapp-profile.yaml为以下内容：

```yaml
### ServiceProfile for webapp.default ###
apiVersion: linkerd.io/v1alpha1
kind: ServiceProfile
metadata:
  name: webapp.default.svc.cluster.local
  namespace: linkerd
spec:
  routes:
  - name: '/books' # This is used as the value for the rt_route label
    condition:
      method: POST
      pathRegex: '/books'
  - name: '/books/{id}' # This is used as the value for the rt_route label
    condition:
      method: GET
      pathRegex: '/books/\d+'
```
 
此服务描述了 webapp 服务响应的两个 route，`/books` 以及`/books/<id>`。我们通过 kubectl apply添加服务配置文件：

```bash
$ kubectl apply -f webapp-profile.yaml
```

在大约一分钟内（Prometheus定期从代理中收集指标）每route metrics将可用于该服务：

```bash
$ linkerd routes svc/webapp
ROUTE         SERVICE   SUCCESS      RPS   LATENCY_P50   LATENCY_P95   LATENCY_P99
/books/{id}    webapp   100.00%   0.3rps          26ms          75ms          95ms
/books         webapp    56.25%   0.5rps          25ms         320ms         384ms
[UNKNOWN]      webapp    79.14%   4.6rps          29ms         165ms         193ms
```
 
现在我们可以轻松地看到每条route的成功率，每秒请求数和延迟，并且我们避免了有关时间序列基数的任何问题。成功！我们还可以看到有些请求与我们定义的任何route都不匹配，这表明我们可能需要添加更多route定义。

### 结论

在这篇文章中，我们展示了如何通过使用Linkerd 2.1中称为service profile的新功能来为服务启用每route（也称为每path）metrics。通过向Linkerd提供有关您的服务所期望的route的一些信息，您可以超越“我的服务失败”，达到“我的服务大部分都很好，除了这个特定的调用是失败的” - 在运行时调试中向前迈出了一大步。

Service Profile不仅适用于基于route的metrics。它们还将成为 Linkerd 路线图中许多功能的基础。将来，您将能够在Service Profile和route上指定其他属性，例如可重试性，速率限制，超时等。在即将发布的Linkerd版本中查找这些功能以及更多功能！
