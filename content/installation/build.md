---
date: 2019-03-03T21:00:00+08:00
title: Build
menu:
  main:
    parent: "installation"
weight: 204
description : "Linkerd2 build"
---

### build

在根目录下执行 `make` 命令，第一次build会非常慢，因为要下载大量依赖：

```bash
$ make
cargo fetch --locked
    Updating git repository `https://github.com/hawkw/prost`
    Updating git repository `https://github.com/seanmonstar/webpki`
    Updating crates.io index
    Updating git repository `https://github.com/carllerche/better-future`
    Updating git repository `https://github.com/linkerd/linkerd2-proxy-api`
    Updating git repository `https://github.com/carllerche/tokio-connect`
    Updating git repository `https://github.com/tower-rs/tower-http`
    Updating git repository `https://github.com/tower-rs/tower`
    Updating git repository `https://github.com/tower-rs/tower-grpc`
    Updating git repository `https://github.com/bluejekyll/trust-dns`
    Updating git repository `https://github.com/carllerche/codegen`
  Downloaded tokio-threadpool v0.1.11
  Downloaded futures v0.1.23
  Downloaded unreachable v1.0.0
  Downloaded mio-uds v0.6.7
  Downloaded bytes v0.4.11
  Downloaded wincolor v0.1.6
  Downloaded crc v1.7.0
  Downloaded httparse v1.3.2
  Downloaded backtrace v0.3.5
  Downloaded chrono v0.4.0
  Downloaded lru-cache v0.1.1
  Downloaded void v1.0.2
  Downloaded miow v0.2.1
  Downloaded unicode-normalization v0.1.5
  Downloaded winapi v0.2.8
  Downloaded syn v0.14.2
  Downloaded arrayvec v0.4.7
  Downloaded try-lock v0.2.1
  Downloaded tokio-current-thread v0.1.4
  Downloaded aho-corasick v0.6.4
  Downloaded tokio-rustls v0.9.0
  Downloaded which v2.0.0
  Downloaded fixedbitset v0.1.8
  Downloaded rand_core v0.2.0
  Downloaded bitflags v1.0.1
  Downloaded slab v0.4.1
  Downloaded widestring v0.2.2
  Downloaded untrusted v0.6.2
  Downloaded gzip-header v0.1.2
  Downloaded unicode-xid v0.0.4
  Downloaded sct v0.5.0
  Downloaded atty v0.2.6
  Downloaded rand_hc v0.1.0
  Downloaded winutil v0.1.1
  Downloaded itoa v0.4.1
  Downloaded semver v0.9.0
  Downloaded multimap v0.4.0
  Downloaded nom v2.2.1
  Downloaded deflate v0.7.18
  Downloaded regex-syntax v0.6.0
  Downloaded libc v0.2.48
  Downloaded rand_pcg v0.1.1
  Downloaded rustc_version v0.2.3
  Downloaded semver-parser v0.7.0
  Downloaded crossbeam-deque v0.6.3
  Downloaded crossbeam-epoch v0.7.1
  Downloaded lazy_static v1.2.0
  Downloaded memoffset v0.2.1
  Downloaded nodrop v0.1.12
  Downloaded lock_api v0.1.5
  Downloaded smallvec v0.6.3
  Downloaded stable_deref_trait v1.1.1
  Downloaded crossbeam-channel v0.3.8
  Downloaded num_cpus v1.8.0
  Downloaded log v0.4.6
  Downloaded tokio-reactor v0.1.1
  Downloaded mio v0.6.14
  Downloaded lazycell v0.6.0
  Downloaded net2 v0.2.32
  Downloaded iovec v0.1.2
  Downloaded winapi-build v0.1.1
  Downloaded ws2_32-sys v0.2.1
  Downloaded tokio-io v0.1.11
  Downloaded byteorder v1.3.1
  Downloaded tokio-codec v0.1.1
  Downloaded tokio-tcp v0.1.0
  Downloaded tokio-fs v0.1.5
  Downloaded tokio-udp v0.1.0
  Downloaded tokio-timer v0.2.10
  Downloaded fnv v1.0.6
  Downloaded enum_primitive v0.1.1
  Downloaded cfg-if v0.1.2
  Downloaded num-traits v0.2.0
  Downloaded adler32 v1.0.2
  Downloaded num-traits v0.1.43
  Downloaded build_const v0.2.0
  Downloaded quickcheck v0.8.0
  Downloaded futures-cpupool v0.1.8
  Downloaded string v0.1.0
  Downloaded time v0.1.39
  Downloaded want v0.0.6
  Downloaded num-iter v0.1.35
  Downloaded redox_syscall v0.1.37
  Downloaded num v0.1.42
  Downloaded termion v1.5.1
  Downloaded num-integer v0.1.36
  Downloaded crossbeam v0.6.0
  Downloaded scopeguard v0.3.3
  Downloaded owning_ref v0.4.0
  Downloaded kernel32-sys v0.2.2
  Downloaded tokio-uds v0.2.5
  Downloaded indexmap v1.0.2
  Downloaded env_logger v0.5.3
  Downloaded termcolor v0.3.5
  Downloaded redox_termios v0.1.1
  Downloaded inotify-sys v0.1.3
  Downloaded inotify v0.7.0
  Downloaded spin v0.5.0
  Downloaded procinfo v0.4.2
  Downloaded remove_dir_all v0.5.0
  Downloaded base64 v0.10.0
  Downloaded tempfile v3.0.5
  Downloaded cc v1.0.28
  Downloaded heck v0.3.0
  Downloaded either v1.4.0
  Downloaded failure_derive v0.1.1
  Downloaded quote v0.3.15
  Downloaded syn v0.11.11
  Downloaded synom v0.11.3
  Downloaded rustc-demangle v0.1.5
  Downloaded unicode-xid v0.1.0
  Downloaded quote v0.6.3
  Downloaded prost-types v0.4.0
  Downloaded memchr v2.0.1
  Downloaded proc-macro2 v0.4.6
  Downloaded thread_local v0.3.5
  Downloaded tokio-signal v0.2.0
  Downloaded utf8-ranges v1.0.0
  Downloaded ucd-util v0.1.1
  Downloaded linked-hash-map v0.4.2
  Downloaded miniz_oxide v0.1.2
  Downloaded ipconfig v0.1.7
  Downloaded winreg v0.5.0
  Downloaded socket2 v0.3.5
  Downloaded matches v0.1.6
  Downloaded unicode-bidi v0.3.4
  Downloaded error-chain v0.8.1
  Downloaded quick-error v1.2.1
  Downloaded resolv-conf v0.6.0
  Downloaded tokio-executor v0.1.5
  Downloaded rand_core v0.3.0
  Downloaded rand_chacha v0.1.1
  Downloaded rand_isaac v0.1.1
  Downloaded percent-encoding v1.0.1
  Downloaded failure v0.1.1
  Downloaded winapi-x86_64-pc-windows-gnu v0.4.0
  Downloaded flate2 v1.0.1
  Downloaded hostname v0.1.4
  Downloaded ipnet v1.0.0
  Downloaded tokio-sync v0.1.1
  Downloaded synstructure v0.6.1
  Downloaded autocfg v0.1.1
  Downloaded rand_xorshift v0.1.1
  Downloaded cloudabi v0.0.3
  Downloaded fuchsia-zircon-sys v0.3.3
  Downloaded fuchsia-zircon v0.3.3
  Downloaded parking_lot v0.7.1
  Downloaded rand_os v0.1.0
  Downloaded parking_lot_core v0.4.0
  Downloaded crossbeam-utils v0.6.5
  Downloaded http v0.1.15
  Downloaded hyper v0.12.24
  Downloaded itertools v0.7.6
  Downloaded unicode-segmentation v1.2.0
  Downloaded petgraph v0.4.11
  Downloaded url v1.7.0
  Downloaded tokio v0.1.15
  Downloaded rand v0.6.3
  Downloaded rand v0.5.1
  Downloaded h2 v0.1.16
  Downloaded miniz_oxide_c_api v0.1.2
  Downloaded regex v1.0.0
  Downloaded backtrace-sys v0.1.16
  Downloaded idna v0.1.4
  Downloaded rustls v0.15.1
  Downloaded winapi v0.3.6
  Downloaded winapi-i686-pc-windows-gnu v0.4.0
  Downloaded ring v0.14.6
  Downloaded 168 crates (19.1 MB) in 16m 37s (largest was `ring` at 5.4 MB)
cargo build --frozen 
   Compiling semver-parser v0.7.0
   Compiling libc v0.2.48
   Compiling cc v1.0.28
   Compiling unicode-xid v0.0.4
   Compiling autocfg v0.1.1
   Compiling quote v0.3.15
   Compiling num-traits v0.2.0
   Compiling cfg-if v0.1.2
   Compiling rustc-demangle v0.1.5
   Compiling byteorder v1.3.1
   Compiling rand_core v0.3.0
   Compiling void v1.0.2
   Compiling unicode-xid v0.1.0
   Compiling stable_deref_trait v1.1.1
   Compiling nodrop v0.1.12
   Compiling lazy_static v1.2.0
   Compiling either v1.4.0
   Compiling memoffset v0.2.1
   Compiling scopeguard v0.3.3
   Compiling termcolor v0.3.5
   Compiling fixedbitset v0.1.8
   Compiling remove_dir_all v0.5.0
   Compiling lazycell v0.6.0
   Compiling futures v0.1.23
   Compiling unicode-segmentation v1.2.0
   Compiling slab v0.4.1
   Compiling build_const v0.2.0
   Compiling indexmap v1.0.2
   Compiling itoa v0.4.1
   Compiling multimap v0.4.0
   Compiling matches v0.1.6
   Compiling fnv v1.0.6
   Compiling spin v0.5.0
   Compiling httparse v1.3.2
   Compiling unicode-normalization v0.1.5
   Compiling untrusted v0.6.2
   Compiling percent-encoding v1.0.1
   Compiling try-lock v0.2.1
   Compiling string v0.1.0
   Compiling rand_core v0.2.0
   Compiling bitflags v1.0.1
   Compiling nom v2.2.1
   Compiling quick-error v1.2.1
   Compiling linked-hash-map v0.4.2
   Compiling linkerd2-never v0.1.0 (/home/sky/work/code/linkerd/src/github.com/linkerd/linkerd2-proxy/lib/never)
   Compiling regex v1.0.0
   Compiling ucd-util v0.1.1
   Compiling adler32 v1.0.2
   Compiling utf8-ranges v1.0.0
   Compiling ipnet v1.0.0
   Compiling log v0.4.6
   Compiling unreachable v1.0.0
   Compiling synom v0.11.3
   Compiling owning_ref v0.4.0
   Compiling proc-macro2 v0.4.6
   Compiling arrayvec v0.4.7
   Compiling rand_xorshift v0.1.1
   Compiling rand_isaac v0.1.1
   Compiling rand_hc v0.1.0
   Compiling crossbeam-utils v0.6.5
   Compiling rand_chacha v0.1.1
   Compiling rand v0.6.3
   Compiling semver v0.9.0
   Compiling itertools v0.7.6
   Compiling unicode-bidi v0.3.4
   Compiling crc v1.7.0
   Compiling petgraph v0.4.11
   Compiling num-integer v0.1.36
   Compiling num-traits v0.1.43
   Compiling heck v0.3.0
   Compiling codegen v0.1.0 (https://github.com/carllerche/codegen#2d4dcc96)
   Compiling backtrace-sys v0.1.16
   Compiling ring v0.14.6
   Compiling smallvec v0.6.3
   Compiling thread_local v0.3.5
   Compiling lru-cache v0.1.1
   Compiling regex-syntax v0.6.0
   Compiling lock_api v0.1.5
   Compiling syn v0.11.11
   Compiling num-iter v0.1.35
   Compiling crossbeam-epoch v0.7.1
   Compiling enum_primitive v0.1.1
   Compiling rustc_version v0.2.3
   Compiling quote v0.6.3
   Compiling crossbeam-channel v0.3.8
   Compiling idna v0.1.4
   Compiling tokio-executor v0.1.5
   Compiling tower-service v0.2.0 (https://github.com/tower-rs/tower#bdecb337)
   Compiling tower-direct-service v0.1.0 (https://github.com/tower-rs/tower#bdecb337)
   Compiling tokio-sync v0.1.1
   Compiling want v0.0.6
   Compiling futures-watch v0.1.0 (https://github.com/carllerche/better-future#07baa13e)
   Compiling futures-mpsc-lossy v0.1.0 (/home/sky/work/code/linkerd/src/github.com/linkerd/linkerd2-proxy/lib/futures-mpsc-lossy)
   Compiling num v0.1.42
   Compiling base64 v0.10.0
   Compiling rand_pcg v0.1.1
   Compiling parking_lot_core v0.4.0
   Compiling procinfo v0.4.2
   Compiling crossbeam-deque v0.6.3
   Compiling syn v0.14.2
   Compiling tower-discover v0.1.0 (https://github.com/tower-rs/tower#bdecb337)
   Compiling tower-in-flight-limit v0.1.0 (https://github.com/tower-rs/tower#bdecb337)
   Compiling tower-util v0.1.0 (https://github.com/tower-rs/tower#bdecb337)
   Compiling rand_os v0.1.0
   Compiling iovec v0.1.2
   Compiling time v0.1.39
   Compiling atty v0.2.6
   Compiling num_cpus v1.8.0
   Compiling net2 v0.2.32
   Compiling memchr v2.0.1
   Compiling hostname v0.1.4
   Compiling inotify-sys v0.1.3
   Compiling socket2 v0.3.5
   Compiling rand v0.5.1
   Compiling linkerd2-stack v0.1.0 (/home/sky/work/code/linkerd/src/github.com/linkerd/linkerd2-proxy/lib/stack)
   Compiling tokio-current-thread v0.1.4
   Compiling tokio-timer v0.2.10
   Compiling tower-buffer v0.1.0 (https://github.com/tower-rs/tower#bdecb337)
   Compiling gzip-header v0.1.2
   Compiling bytes v0.4.11
   Compiling tower-reconnect v0.1.0 (https://github.com/tower-rs/tower#bdecb337)
   Compiling futures-cpupool v0.1.8
   Compiling url v1.7.0
   Compiling synstructure v0.6.1
   Compiling mio v0.6.14
   Compiling chrono v0.4.0
   Compiling resolv-conf v0.6.0
   Compiling aho-corasick v0.6.4
   Compiling linkerd2-router v0.1.0 (/home/sky/work/code/linkerd/src/github.com/linkerd/linkerd2-proxy/lib/router)
   Compiling deflate v0.7.18
   Compiling tower-retry v0.1.0 (https://github.com/tower-rs/tower#bdecb337)
   Compiling prost v0.4.0 (https://github.com/hawkw/prost?branch=s/tempdir/tempfile#1115619a)
   Compiling tokio-io v0.1.11
   Compiling http v0.1.15
   Compiling failure_derive v0.1.1
   Compiling tower-balance v0.1.0 (https://github.com/tower-rs/tower#bdecb337)
   Compiling mio-uds v0.6.7
   Compiling env_logger v0.5.3
   Compiling webpki v0.19.1 (https://github.com/seanmonstar/webpki?branch=cert-dns-names#aae34c01)
   Compiling sct v0.5.0
   Compiling tokio-reactor v0.1.1
   Compiling tokio-codec v0.1.1
   Compiling tokio-connect v0.1.0 (https://github.com/carllerche/tokio-connect#f7ad1ca4)
   Compiling rustls v0.15.1
   Compiling tokio-udp v0.1.0
   Compiling tokio-uds v0.2.5
   Compiling tokio-tcp v0.1.0
   Compiling tokio-signal v0.2.0
   Compiling linkerd2-timeout v0.1.0 (/home/sky/work/code/linkerd/src/github.com/linkerd/linkerd2-proxy/lib/timeout)
   Compiling tempfile v3.0.5
   Compiling quickcheck v0.8.0
   Compiling parking_lot v0.7.1
   Compiling backtrace v0.3.5
   Compiling failure v0.1.1
   Compiling crossbeam v0.6.0
   Compiling which v2.0.0
   Compiling prost-derive v0.4.0 (https://github.com/hawkw/prost?branch=s/tempdir/tempfile#1115619a)
   Compiling trust-dns-proto v0.6.0 (https://github.com/bluejekyll/trust-dns?rev=7c8a0739dad495bf5a4fddfe86b8bbe2aa52d060#7c8a0739)
   Compiling tokio-threadpool v0.1.11
   Compiling prost-build v0.4.0 (https://github.com/hawkw/prost?branch=s/tempdir/tempfile#1115619a)
   Compiling tower-add-origin v0.1.0 (https://github.com/tower-rs/tower-http#3599ce02)
   Compiling h2 v0.1.16
   Compiling tower-http v0.1.0 (https://github.com/tower-rs/tower-http#3599ce02)
   Compiling tokio-fs v0.1.5
   Compiling tokio v0.1.15
   Compiling inotify v0.7.0
   Compiling linkerd2-task v0.1.0 (/home/sky/work/code/linkerd/src/github.com/linkerd/linkerd2-proxy/lib/task)
   Compiling prost-types v0.4.0 (https://github.com/hawkw/prost?branch=s/tempdir/tempfile#1115619a)
   Compiling prost-types v0.4.0
   Compiling trust-dns-resolver v0.10.2 (https://github.com/bluejekyll/trust-dns?rev=7c8a0739dad495bf5a4fddfe86b8bbe2aa52d060#7c8a0739)
   Compiling tokio-rustls v0.9.0
   Compiling linkerd2-fs-watch v0.1.0 (/home/sky/work/code/linkerd/src/github.com/linkerd/linkerd2-proxy/lib/fs-watch)
   Compiling hyper v0.12.24
   Compiling tower-grpc v0.1.0 (https://github.com/tower-rs/tower-grpc#3fd94dcf)
   Compiling tower-grpc-build v0.1.0 (https://github.com/tower-rs/tower-grpc#3fd94dcf)
   Compiling linkerd2-proxy-api v0.1.6 (https://github.com/linkerd/linkerd2-proxy-api?tag=v0.1.6#ff6064f8)
   Compiling hyper-balance v0.1.0 (/home/sky/work/code/linkerd/src/github.com/linkerd/linkerd2-proxy/lib/hyper-balance)
   Compiling linkerd2-metrics v0.1.0 (/home/sky/work/code/linkerd/src/github.com/linkerd/linkerd2-proxy/lib/metrics)
   Compiling linkerd2-proxy v0.1.0 (/home/sky/work/code/linkerd/src/github.com/linkerd/linkerd2-proxy)
    Finished dev [unoptimized] target(s) in 1m 06s
```

之后再执行就很快了：

```bash
 $ make
cargo fetch --locked
cargo build --frozen 
    Finished dev [unoptimized] target(s) in 0.15s
```

