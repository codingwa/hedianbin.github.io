---
layout: page
title: Collections
permalink: /collection/
icon: bookmark
type: page
---

* content
{:toc}
# 超有价值的Golang文章

文章排名不分先后， 文章也不一定完全按照日期来排列。我按照文章的大致内容分了类，便于查找。

文章主要从golangweekly、gocn每日新闻、medium、reddit、twitter、、知名博主的文章搜集而来。如果你发现好的2017年的Go文章没有列出来，欢迎在评论中粘帖出来，我会加入到文章正文中。

本文主要列出的是文章，2018年也涌现出来很多优秀的库和工具，但是不是本文要介绍的内容，所以没有列出来。



![20180918153726788551180.png](http://p9ug71a1p.bkt.clouddn.com/20180918153726788551180.png)

## 语言规范

- [Close Channels Gracefully in Golang](http://www.tapirgames.com/blog/golang-channel-closing): 如何优雅地关闭channel?
- [Compile-time assertions in Go](http://commaok.xyz/post/compile-time-assertions/): 编译时断言
- [Why are slices sometimes altered when passed by value in Go?](https://www.calhoun.io/why-are-slices-sometimes-altered-when-passed-by-value-in-go/): Go不是按值传递么，怎么slice传入后被更改了呢？其实map也一样
- [Bit Hacking with Go](https://dev.to/vladimirvivien/bit-hacking-with-go): Go的位操作
- [Go Range Loop Internals](https://garbagecollected.org/2017/02/22/go-range-loop-internals/): range内幕
- [Blocks and Scopes in Golang](http://www.tapirgames.com/blog/golang-block-and-scope): 代码块和作用域
- [Address Alignments in Go](http://www.tapirgames.com/blog/golang-memory-alignment):地址对齐
- [Understand Go pointers in less than 800 words](https://dave.cheney.net/2017/04/26/understand-go-pointers-in-less-than-800-words-or-your-money-back):回顾一下指针的概念
- [Another Introduction to Pointers in Go](https://golangbot.com/pointers/):另一篇关于指针的介绍
- [There is no pass-by-reference in Go](https://dave.cheney.net/2017/04/29/there-is-no-pass-by-reference-in-go): Go中没有引用传值
- [Why golang garbage-collector not implement Generational and Compact gc?](https://groups.google.com/forum/#!msg/golang-nuts/KJiyv2mV2pU/wdBUH1mHCAAJ): 为什么不使用分代或者压缩的垃圾回收器？
- [Modern garbage collection](https://blog.plan99.net/modern-garbage-collection-911ef4f8bd8e): Go的垃圾回收器
- [Language Mechanics On Stacks And Pointers](https://www.goinggo.net/2017/05/language-mechanics-on-stacks-and-pointers.html): William Kennedy写的系列， 一共四篇， 深入底层
- [Structures](https://golangbot.com/shttps://medium.com/golangspec/init-functions-in-go-eac191b3860atructs/): Go struct介绍
- [Go调度详解](https://zhuanlan.zhihu.com/p/27056944): 调度概览
- [也谈goroutine调度器](http://tonybai.com/2017/06/23/an-intro-about-goroutine-scheduler/): 也谈谈Go的调度器
- [了解 Go 1.9 的类型别名](http://colobu.com/2017/06/26/learn-go-type-aliases/): Go 1.9中的类型别名
- [Golang Internals Part 2: Nice benefits of named return values](https://blog.minio.io/golang-internals-part-2-nice-benefits-of-named-return-values-1e95305c8687): 返回值采用命名方式的好处
- [Go Interface 源码剖析](http://legendtkl.com/2017/07/01/golang-interface-implement/): interface源代码
- [Survey of Rounding Implementations in Go](https://www.cockroachlabs.com/blog/rounding-implementations-in-go/): 四舍五入不简单
- [Golang 热更新研究笔记](http://www.cppblog.com/sunicdavy/archive/2017/07/06/215057.html): 热更新笔记
- [go empty interface](https://flaviocopes.com/go-empty-interface/):空接口
- [理解 go interface 的 5 个关键点](http://sanyuesha.com/2017/07/22/how-to-understand-go-interface/): 接口关键点
- [Go汇编实战的坑](https://mzh.io/golang-asm-notes): 赞，国人给golang提patch
- [golang语言编译的二进制可执行文件为什么比 C 语言大](http://www.cnxct.com/why-golang-elf-binary-file-is-large-than-c/):深入分析
- [init functions in Go](https://medium.com/golangspec/init-functions-in-go-eac191b3860a):init函数不简单
- [Efficient Bit Manipulation in Go 1.9](http://herman.asia/bit-manipulation-in-go-1-9): 有效的位操作
- [Learn Go constants — A visual guide](https://blog.learngoprogramming.com/learn-golang-typed-untyped-constants-70b4df443b61):可视化系列
- [Go cheatsheet](https://devhints.io/go):小抄
- [defer in go](https://blog.learngoprogramming.com/gotchas-of-defer-in-go-1-8d070894cb01): 还有第二部分 [part2](https://blog.learngoprogramming.com/5-gotchas-of-defer-in-go-golang-part-ii-cc550f6ad9aa)
- [Using named return variables to capture panics in Go](https://www.calhoun.io/using-named-return-variables-to-capture-panics-in-go/): 返回panic的错误
- [Go Reflection: Creating Objects from Types](https://medium.com/kokster/go-reflection-creating-objects-from-types-part-i-primitive-types-6119e3737f5d):基本类型， [part2](https://medium.com/kokster/go-reflection-creating-objects-from-types-part-ii-composite-types-69a0e8134f20)其它类型

## 测试

- [Go advanced testing tips & tricks](https://medium.com/@povilasve/go-advanced-tips-tricks-a872503ac859): Go测试提示和技巧
- [5 simple tips and tricks for writing unit tests in #golang](https://medium.com/@matryer/5-simple-tips-and-tricks-for-writing-unit-tests-in-golang-619653f90742):写测试的5个技巧
- [5 Advanced Testing Techniques in Go](https://segment.com/blog/5-advanced-testing-techniques-in-go/): 5个测试的高级技术

## 调试和性能调优

- [Profiling and optimizing Go web applications](http://artem.krylysov.com/blog/2017/03/13/profiling-and-optimizing-go-web-applications/): 优化web程序，使用profiler
- [go tool trace](https://making.pusher.com/go-tool-trace/): trace工具介绍的很少，这是其中一篇
- [Debugging Go core dumps](https://rakyll.org/coredumps/):使用core dump调试
- [Benchmarking Go programs](https://scene-si.org/2017/06/06/benchmarking-go-programs/): 测试和pprof, [第二部分](https://scene-si.org/2017/07/07/benchmarking-go-programs-part-2/)
- [GO APP MONITORING: EXPVAR, PROMETHEUS AND STATSD](https://www.opsdash.com/blog/golang-app-monitoring-statsd-expvar-prometheus.html): 监控实践
- [一个内部API系统的性能优化](https://zhuanlan.zhihu.com/p/27211445): 国内用户的优化实践
- [Profiler labels in Go](https://rakyll.org/profiler-labels/):不常用但是有用的技巧
- [diagnostics](https://tip.golang.org/doc/diagnostics.html): 官方的介绍更准确，诊断。
- [Go's work-stealing scheduler](https://rakyll.org/scheduler/):rakyll的介绍
- [3倍性能的go程序优化实践](https://mp.weixin.qq.com/s/9IKaXeWTiiQTFlvZzxgsEA): 国内的优化实践
- [Seven steps to 100x faster](https://syslog.ravelin.com/making-something-faster-56dd6b772b83): 100倍的性能提升
- [A Million WebSockets and Go](https://medium.freecodecamp.org/million-websockets-and-go-cc58418460bb): 百万websocket连接和性能提升经验
- [Golang 与系统调用](https://segmentfault.com/a/1190000010630859): 根据演讲整理
- [Performance comparison of regular expression engines](https://nasciiboy.github.io/raptorVSworld/):正则表达式库的性能比较
- [Allocation Efficiency in High-Performance Go Services](https://segment.com/blog/allocation-efficiency-in-high-performance-go-services/):有效的内存分配
- [使用 bcc/BPF 分析 go 程序](http://colobu.com/2017/09/22/golang-bcc-bpf-function-tracing/): Brendan Gregg的文章
- [Your pprof is showing](http://mmcloughlin.com/posts/your-pprof-is-showing): pprof,不想多说了
- [Using the Go tracer to speed up fractal making](https://campoy.cat/blog/using-the-go-tracer-to-speed-up-fractal-making/)：go tracer
- [Profiling Go](http://www.integralist.co.uk/posts/profiling-go/): 很好profile总结
- [The new pprof user interface](https://rakyll.org/pprof-ui/): go 1.10中已包含新界面
- [High Performance Go](http://talks.godoc.org/github.com/davecheney/qconsf-2017/high-performance-go.slide#1):Dave Cheney在QComSF 2017上的演讲
- [Go Profiler Internals](https://stackimpact.com/blog/go-profiler-internals/):所谓的profile内幕
- [Optimizing GoLang for High Performance with ARM64 Assembly](https://www.slideshare.net/linaroorg/optimizing-golang-for-high-performance-with-arm64-assembly-sfo17314?qid=bfcec716-6947-47e5-b9c1-2bb1c4e5bfdf&v=&b=&from_search=16): 针对ARM64的优化

## 标准库

- [Plugins in Go 1.8](https://nick.groenen.me/posts/2017/01/09/plugins-in-go-18/): 对Go 1.8增加的plugin包的速览
- [TCP/IP Networking](https://appliedgo.net/networking/): 网络编程入门
- [Linux, Netlink, and Go — Part 1: netlink](https://medium.com/@mdlayher/linux-netlink-and-go-part-1-netlink-4781aaeeaca8):访问linux netlink,还有第二篇和第三篇
- [Port Forwarding with Go](https://zupzup.org/go-port-forwarding/): 端口转发
- [Writing Modular Go Programs with Plugins](https://medium.com/learning-the-go-programming-language/writing-modular-go-programs-with-plugins-ec46381ee1a9): 插件
- [Advanced command execution in Go with os/exec](https://blog.kowalczyk.info/article/wOYk/advanced-command-execution-in-go-with-osexec.html):学习os/exec
- [expvar in action](https://orangetux.nl/post/expvar_in_action/):知道这个标准包吗，用过吗？
- [Golang 互斥锁内部实现](https://zhuanlan.zhihu.com/p/27608263): Go的锁实现
- [Go unsafe 包之内存布局](https://mp.weixin.qq.com/s?__biz=MzI3MjU4Njk3Ng==&mid=2247483805&idx=1&sn=f365ae1beb222cb3c271fcc4ee3a1aae&chksm=eb310012dc4689042a9c90faa073996133fb6d98f7b9e222c1828c07a9daf7ab387c3c5b861e&scene=0&key=845f133eeabfe179ef76ee7968a400ec4974d784b308d18080a26e74f43c37788cd79ca4e76fd8ae7fe9b79022de525cc5f59e101f819b00b6f4eee846bcf83df332de87b5773356522d959499f4de94&ascene=0&uin=MjQ2NTU1&devicetype=iMac+MacBookPro11%2C3+OSX+OSX+10.12.5+build(16F73)&version=12020810&nettype=WIFI&fontScale=100&pass_ticket=Dip3QhAoFuCXmvNUJBCqwg2%2FTrorNO8SzZpbc%2FtgnD4%3D): unsafe的一些函数
- [Go 1.9 sync.Map揭秘](http://colobu.com/2017/07/11/dive-into-sync-Map/): sync.Map的实现揭秘
- [Basic AST Traversal in Go](https://zupzup.org/go-ast-traversal/): ast,做工具和代码分析的都需要
- [A Go Programmer's Guide to Syscalls](https://www.youtube.com/watch?v=01w7viEZzXQ&index=20&list=PL2ntRZ1ySWBdD9bru6IR-_WXUgJqvrtx9&utm_source=golangweekly&utm_medium=email):Liz Rice的演讲
- [Pitfalls of context values and how to avoid or mitigate them in Go](https://www.calhoun.io/pitfalls-of-context-values-and-how-to-avoid-or-mitigate-them/): Context的陷阱以及如何避免它
- [The 30 Most Popular Go String Functions](http://www.golangprograms.com/golang/string-functions/): 最常用的strings的30个方法
- [Serving HTTPS](https://pocketgophers.com/serving-https/): 应用https
- [How To Trust Extra CA Certs In Your Go App](https://forfuncsake.github.io/post/2017/08/trust-extra-ca-cert-in-go-app/): 使用外部的CA
- [GOLANG中time.After释放的问题](https://gocn.io/article/403): time.After使用的时候注意事项
- [The new kid in town — Go’s sync.Map](https://medium.com/@deckarep/the-new-kid-in-town-gos-sync-map-de24a6bf7c2c):sync.Map的性能
- [The ultimate guide to writing a Go tool](https://arslan.io/2017/09/14/the-ultimate-guide-to-writing-a-go-tool/): Go工具开发终极手册，gomodifytags的作者
- [Streaming IO in Go](https://medium.com/learning-the-go-programming-language/streaming-io-in-go-d93507931185): io
- [Things to know about HTTP in Go](https://scene-si.org/2017/09/27/things-to-know-about-http-in-go/): Go HTTP
- [Important interfaces that every Go developer should know](https://spino.tech/blog/important-go-interfaces/#golang): 重要接口
- [Go Walkthrough: fmt](https://medium.com/go-walkthrough/go-walkthrough-fmt-55a14bbbfc53): fmt详解
- [Context API explained](https://siadat.github.io/post/context):context详解
- [Basic AST Manipulation in Go](https://zupzup.org/ast-manipulation-go/):还是ast
- [In-depth introduction to bufio.Scanner in Golang](https://medium.com/golangspec/in-depth-introduction-to-bufio-scanner-in-golang-55483bb689b4):深入介绍bufio.Scanner
- [Go Slice vs Maps](https://boltandnuts.wordpress.com/2017/11/20/go-slice-vs-maps/):性能比较
- [Introduction to bufio package in Golang](https://medium.com/golangspec/introduction-to-bufio-package-in-golang-ad7d1877f762): bufio介绍
- [TCP Socket Implementation On Golang](https://medium.com/@ggiovani/tcp-socket-implementation-on-golang-c38b67c5d8b): 从外部推断内部实现
- [Understanding Go programs with go/parser](https://medium.com/@francesc/understanding-go-programs-with-go-parser-c4e88a6edb87): 带你学习go/parser
- [unsafe.Pointer and system calls](https://blog.gopheracademy.com/advent-2017/unsafe-pointer-and-system-calls/):理解 unsafe.Pointer和系统调用
- [Using Go Templates](https://blog.gopheracademy.com/advent-2017/using-go-templates/):使用模板

## 编程模式

- [Functional Options Pattern in Go](https://halls-of-valhalla.org/beta/articles/functional-options-pattern-in-go,54/): 介绍了参数以函数子的方式实现，更出名的是[ Rob Pike](https://commandcenter.blogspot.de/2014/01/self-referential-functions-and-design.html)提出的这种模式，以及[Dave Cheney](https://dave.cheney.net/2014/10/17/functional-options-for-friendly-apis)的进一步介绍。
- [A Story of a Fat Go Binary](https://hackernoon.com/a-story-of-a-fat-go-binary-20edc6549b97): 使用gofat给Go程序瘦身
- [a size profiler for binaries ](https://github.com/google/bloaty):瘦身瘦身，google出品
- [How to ship production-grade Go](https://www.oreilly.com/ideas/how-to-ship-production-grade-go):开发产品级的程序
- [Go programming language secure coding practices guide](https://github.com/Checkmarx/Go-SCP):安全代码指南
- [Error handling patterns in Go](https://mijailovic.net/2017/05/09/error-handling-patterns-in-go/): Go的错误处理总是让人困惑
- [Alternative Patterns for Method Overloading in Go](https://mymemorysucks.wordpress.com/2017/05/16/alternative-patterns-for-method-overloading-in-go/): Go的方法重载的可选实现
- [Go, without package scoped variables](https://dave.cheney.net/2017/06/11/go-without-package-scoped-variables): 不推荐包级别的变量，但是怎么做呢
- [Don’t defer Close() on writable files](https://joeshaw.org/dont-defer-close-on-writable-files/): defer会忽略错误
- [Go Patterns](http://tmrts.com/go-patterns/): Go设计模式
- [Golang Concurrency Tricks](http://udhos.github.io/golang-concurrency-tricks/):channel和goroutine技巧
- [go make 和 new 的区别](http://sanyuesha.com/2017/07/26/go-make-and-new/): 区别
- [Context isn’t for cancellation](https://dave.cheney.net/2017/08/20/context-isnt-for-cancellation): Context目的不是为了cancel
- [Inject build-time variables with Golang](https://blog.alexellis.io/inject-build-time-vars-golang/):
- [go advices](https://github.com/cristaloleg/go-advices): Go建议清单
- [Error handling in Upspin](https://commandcenter.blogspot.com.au/2017/12/error-handling-in-upspin.html): Rob Pike的实践
- [Dynamic JSON in Go](http://eagain.net/articles/go-dynamic-json/): 使用`*json.RawMessage` 和 `interface{}`

## 开发技巧

- [Adding custom data to Go binaries at compile time](https://goenning.net/2017/01/25/adding-custom-data-go-binaries-compile-time/): 编译时给二进制文件加点料
- [Go Quickstart Tips for Experienced Programmers](https://stablekernel.com/go-quickstart-helpful-tips-for-experienced-programmers/): 给经验丰富的程序员的一些Go的小贴士
- [Gracefully terminate a program in Go](http://guzalexander.com/2017/05/31/gracefully-exit-server-in-go.html):优雅滴关闭程序
- [Golang 中使用 JSON 的小技巧](https://gocn.io/article/363): JSON使用的小技巧
- [How to find out which Go version built your binary](https://dave.cheney.net/2017/06/20/how-to-find-out-which-go-version-built-your-binary): debug发现Go版本
- [Generating good unique ids in Go](https://blog.kowalczyk.info/article/JyRZ/generating-good-random-and-unique-ids-in-go.html):产生唯一的ID
- [Fencing off Go Applied - A Practical Look at a Go Research Paper](https://emil.hessman.se/articles/fencing-off-go): 使用 dingo-hunter 和 Gong 编译时检查死锁和channel
- [Go Tooling in Action](https://github.com/campoy/go-tooling-workshop): Go开发时的一些工具介绍
- [How to use conditional compilation with the go build tool](https://dave.cheney.net/2013/10/12/how-to-use-conditional-compilation-with-the-go-build-tool):条件编译
- [Strace in 60 lines of Go](https://hackernoon.com/strace-in-60-lines-of-go-b4b76e3ecd64): 实现strace
- [3 ways to iterate in Go](https://blog.kowalczyk.info/article/1Bkr/3-ways-to-iterate-in-go.html):三种方式遍历
- [RustGo: Calling Rust From Go with Little Overhead](https://golangweekly.com/issues/173):高效的从Go调用Rust
- [Version Constraints and Go](https://medium.com/@theckman/version-constraints-and-go-c9309be15773):指定的Go版本才能编译
- [6 Go Tips You Should (probably not) Use](https://medium.com/@Raedwulf/6-go-tips-you-should-probably-not-use-b252dfd0a3c4): 6个Go小技巧，但是你不应该在代码中使用， :)
- [15 Lessons in Golang](https://blog.thesparktree.com/15-lessons-in-golang):15个经验教训
- [Using functional options instead of method chaining in Go](https://www.calhoun.io/using-functional-options-instead-of-method-chaining-in-go/): 使用函数式参数，而不是链式操作
- [合理配置GOMAXPROCS提升一倍以上的性能](https://my.oschina.net/linker/blog/1504199): 对于I/O密集型的程序，建议设置GOMAXPROCS大一些
- [Lint your #golang code like a mad man!](http://colobu.com/2017/06/27/Lint-your-golang-code-like-a-mad-man/): 像牛人一样编写代码
- [Rate Limiting Service Calls in Go](https://medium.com/@KevinHoffman/rate-limiting-service-calls-in-go-3771c6b7c146):限流
- [Sending HTML email using Go](http://www.blog.labouardy.com/sending-html-email-using-go/):发送html邮件
- [Go Assembly 示例](http://colobu.com/goasm/): Go使用汇编的例子
- [go web examples](https://gowebexamples.com/):web开发例子
- [Handling CORS In A Golang Web Application](https://www.thepolyglotdeveloper.com/2017/10/handling-cors-golang-web-application/):CORS处理
- [Understanding Go panic output](https://joeshaw.org/understanding-go-panic-output/): 理解Go的panic输出
- [Go语言中实现基于 event-loop 网络处理](http://colobu.com/2017/11/29/event-loop-networking-in-Go/):不同的思路
- [Generating Go structs from JSON](https://pocketgophers.com/go-structs-from-json/):5个json to gostruct的工具
- [go语言死循环分析](http://michaelyou.github.io/2017/12/05/go%E8%AF%AD%E8%A8%80%E6%AD%BB%E5%BE%AA%E7%8E%AF%E5%88%86%E6%9E%90%2f):当然不是go的bug,看看go的[讨论](https://github.com/golang/go/issues/15442)
- [Access unexported fields in golang/reflect](https://stackoverflow.com/questions/42664837/access-unexported-fields-in-golang-reflect): 访问unexported的字段
- [Why is port a string and not an integer](https://stackoverflow.com/questions/47992477/why-is-port-a-string-and-not-an-integer): `URL.Port`为什么是字符串而不是整数？
- [golang: How the select worked when multiple channel involved](https://stackoverflow.com/questions/47645808/golang-how-the-select-worked-when-multiple-channel-involved):使用伪随机
- [How to reuse request body of *http.Request](https://stackoverflow.com/questions/47625304/how-to-reuse-request-body-of-http-request-between-http-middleware-handlers): request.Body读完就不能再多了，如何重用？

## 面试题

- [golang 面试题](https://zhuanlan.zhihu.com/p/26972862): 诺唯总结的11个面试题， [解答](https://yushuangqi.com/blog/2017/golang-mian-shi-ti-da-an-yujie-xi.html)
- [Golang精编100题](https://www.jianshu.com/p/f690203ff168):选择题和填空题，试试看

## 第三方库

- [Infinite Sets in Go](https://blog.haardiek.org/infinite-sets-in-go.html): 把set实现玩出花
- [awesome-go-storage](https://github.com/gostor/awesome-go-storage): 100多个Go语言实现的存储引擎，强不强大？
- [Learn Web Programming in Go by Examples](https://gowebexamples.com/): 类似gobyexamples,这次奉上的是关于web开发的
- [Visualizing Concurrency in Go](https://divan.github.io/posts/go_concurrency_visualize/):可视化goroutine
- [Network Protocol Breakdown: Ethernet and Go](https://medium.com/@mdlayher/network-protocol-breakdown-ethernet-and-go-de985d726cc1): 学习底层网络协议处理
- [再谈谈获取 goroutine id 的方法](http://colobu.com/2017/08/04/talk-about-getting-goroutine-id-again/): goroutine ID的获得，hacker方式
- [Listen gRPC and HTTP requests on the same port](https://medium.com/@drgarcia1986/listen-grpc-and-http-requests-on-the-same-port-263c40cb45ff): grpc和http共用端口
- [gRPC-Go performance Improvements](https://grpc.io/2017/08/22/grpc-go-perf-improvements.html): gRPC近期的性能优化
- [Building Blockchain in Go](https://jeiwan.cc/posts/building-blockchain-in-go-part-1/):使用Go编写区块链程序系列
- [Minimal Perfect Hash Functions](https://blog.gopheracademy.com/advent-2017/mphf/):完美的hash函数，极小

## 应用

- [今日头条Go建千亿级微服务的实践](https://36kr.com/p/5073181.html): 今日头条的微服务

## 版本新特性

- [5 things to watch in Go programming in 2017](https://hackernoon.com/5-things-to-watch-in-go-programming-in-2017-39cd7a7e58e3): G0 1.8新的特性介绍
- [Go 1.8](https://blog.gopheracademy.com/advent-2016/go-1.8/): 2016最后一天写的，也算做2017年的文章吧
- [The State of Go - February 2017](https://talks.golang.org/2017/state-of-go.slide): 2017对Go的综评，来自Go团队
- [What is new in Go 1.8](https://www.slideshare.net/huazhihao1/what-is-new-in-go-18-72210978): what's new?
- [Logging, interfaces, and allocation](http://commaok.xyz/post/interface-allocs/): Go 1.9中的一点优化
- [HTTP/2 Server Push](https://blog.golang.org/h2push): http2 push功能
- [The State of Go - May 2017](https://talks.golang.org/2017/state-of-go-may.slide#1): The State of Go系列
- [What’s new in Google’s Go 1.9](https://www.infoworld.com/article/3201037/application-development/whats-new-in-googles-go-language.html): what's new
- [Toward Go 2](https://blog.golang.org/toward-go2): 目标Go2
- [Go 1.9中值得关注的几个变化](http://tonybai.com/2017/07/14/some-changes-in-go-1-9/): what's new in go 1.9
- [improve timers scalability on multi-CPU systems](https://go-review.googlesource.com/c/go/+/34784): go 1.10提升timer性能
- [Go 1.10 Release Notes](https://tip.golang.org/doc/go1.10): go 1.10先睹为快
- [Golang 1.10 前瞻](https://zhuanlan.zhihu.com/p/31820378): 新特性

# 超有价值的区块链文章

