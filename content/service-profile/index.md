---
date: 2018-11-21T20:20:00+08:00
title: Service Profile
weight: 1000
description : "Service Profile"
---

## 介绍

### Linkerd 2.1 版本release note

这是来自 linkerd 2.1 版本release note的介绍。

[Announcing Linkerd 2.1](https://blog.linkerd.io/2018/12/06/announcing-linkerd-2-1/) ：

Service Profiles:

Linkerd 2.1引入了 service profile 的概念，这是一种向Linkerd提供服务信息的轻量级方法。这些信息包括服务的路由，如预期响应的API调用，以及Linkerd应如何处理这些路由。 （作为旁注，service profile 是作为 Kubernetes CRD 实现的，将Linkerd创建的Kubernetes CRD的总数增加到1。）

服务配置文件是一个非常令人兴奋的扩展，因为它们为项目提供了基本构建块：能够在每个服务的基础上配置Linkerd的行为。在即将发布的版本中，我们将添加许多基于 service profile 的功能，包括重试，熔断，速率限制和超时。

Service Profile 也是Linkerd 2.x背后设计理念的一个很好的证明。通过在服务级别而不是全局级别附加配置，我们确保Linkerd可以继续逐步采用 - 一次一个服务。当然，即使没有指定service profile，Linkerd仍然可以开箱即用。

## 产品规划

暂时还只是用于metrics，不过未来会用在更多的领域：

Service profiles aren’t just for route-based metrics. They will also serve as the foundation for many of the features on Linkerd’s roadmap. In the future, you will be able to specify other properties on service profiles and routes such as retryability, rate limits, timeouts, etc. Look for these features and more in upcoming Linkerd releases!

> Service profile不仅适用于基于路由的metrics。 它们还将成为Linkerd路线图中许多功能的基础。将来，您将能够在Service profile和路由上指定其他属性，例如可重试性，速率限制，超时等。在即将发布的Linkerd版本中查找这些功能以及更多功能！


## 背景

[Introduce Service Profiles for basic policy configuration](https://github.com/linkerd/linkerd2/issues/1418)

### 问题

构建适用于所有类型的 HTTP 服务的解决方案是不可能的。此外，正如我们从Linkerd配置中了解到的那样，描述一个具有足够细节的服务可能会很麻烦。

但是，即使预先对服务了解一点点，也可以让我们大幅增强服务，而无需用户提供详细配置。

例如，对我们来说为所有`:paths`记录遥测数据是不切实际的，因为合法服务有很多方法可以让我们的metrics系统超载。（例如，考虑GitHub URL - 我们不希望存储在GitHub中每个SHA的度量。）相反，如果我们知道服务是gRPC服务，那么记录每个路径的metrics变得容易和明显（因为
gRPC服务具有小的，受限制的路径数量）。

类似地，由于没有关于请求可重试性的协议级提示，因此不可能假设gRPC 服务的任何幂等语义。
另一方面，如果我们知道服务是一个条件良好的 REST 服务，我们可以根据请求的性质
（HTTP method和有效负载大小/种类）轻松推断可重试性。

### 目标

1. 允许用户提供最少量的配置，这些配置可以支持最大的运维效益。
2. 建立一个原型，以便以后可以扩展支持更丰富更详细的策略。

#### 非目标

1. 支持任意复杂服务的任意策略。

### 提案：Service Profile

Linkerd控制器已修改为可以获知一组Service Profile。每个Kubernetes `Service`可能与Profile相关联。控制器通过Destination API等使用这些Profile来通知代理它的行为。

每个Profile都描述了服务的预期行为方式。最初，我们只支持一个或两个非常粗略的适用于大量应用程序的Profile。稍后，我们可以添加扩展这些固定Profile的功能，或创建自定义Profile。

### 例子：

#### `grpc`

- 每路径指标/Per-path metrics
- 失败分类/Failure classification

#### `http-rest`

- 重试/retries
- 失败分类/Failure classification

### 开放问题

- 如何将Profile附加到服务？Annotation？CRD？
- 用户如何设置这些Profile？ `inject`到服务上？专职的命令？