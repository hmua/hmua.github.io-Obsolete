
# 随手记点笔记
### Rust
* TiDB是用Rust开发的开源*分布式数据*引擎
#### 文一(http://blog.sina.com.cn/s/blog_66e4c9bd0102wfz4.html)
>1. tidb在tikv上层，tidb用go实现，tikv用rust实现
>2. tidb的目标是实现google的F1，tikv和google的spanner有关系
>3. sql语句解析成AST（Abstract syntax trees），使用optimizer转换为查询计划，并对查询计划进行逻辑优化（logic.go）和物理优化，变成Plan tree，再由executor转换成Executor tree
>4. F1是google开发的一个可容错可扩展的RDBMS，基于spanner（重要的底层存储技术），支持sql。
>5. spanner致力于跨数据中心的数据复制，也提供数据库功能，是一个“临时多版本”的数据库。数据存储在一个版本化的关系表里，数据会根据其提交的时间打上时间错。spanner有一个全球时间同步机制，可以在数据提交时给出一个时间戳，保证了外部一致性。
>6. 启动顺序：PD->TIKV->TIDB。假定有3个node，每个node依次启动PD、TIKV，在node1启动TIDB，然后可以用mysql client连接TIDB。PD(placement driver)负责管理协调TIKV cluster。
#### 文二(http://www.infoq.com/cn/news/2017/09/Select-minority-language-Rust-Ti)
>Rust 是由 Mozilla 研究室主导开发的一门现代系统编程语言，自 2015 年 5 月发布 1.0 之后，一直以每 6 周一个小版本的开发进度稳定向前推进。语言设计上跟 C++ 一样强调零开销抽象和 RAII。
>>RAII是resource acquisition is initialization的缩写，意为“资源获取即初始化”。它是C++之父Bjarne Stroustrup提出的设计理念，其核心是把资源和对象的生命周期绑定，对象创建获取资源，对象销毁释放资源。在RAII的指导下，C++把底层的资源管理问题提升到了对象生命周期管理的更高层次。(http://www.cppblog.com/aaxron/archive/2011/03/22/142475.html)
>
>拥有极小的运行时和高效的 C 绑定，使其运行效率与 C/C++ 一个级别，非常适合对性能要求较高的系统编程领域。利用强大的类型系统和独特的生命周期管理实现了编译期内存管理，保证内存安全和线程安全的同时使编译后的程序运行速度极快，Rust 还提供函数式编程语言的模式匹配和类型推导，让程序写起来更简洁优雅。宏和基于 trait 的泛型机制让 Rust 的拥有非常强大的抽象能力，在实际工程中尤其是库的编写过程中可以少写很多 boilerplate 代码。
>>trait 可以与泛型结合来将泛型限制为拥有特定行为的类型，而不是任意类型。(https://kaisery.github.io/trpl-zh-cn/ch10-00-generics.html)
>##  Rust 的生态
>Rust 由于有 Cargo 这样一个非常出色的包管理工具，周边的第三方库发展非常迅速，各个领域都有比较成熟的库，比如 HTTP 库有 Hyper，异步 IO 库有 Tokio, mio 等，基本上构建后端应用必须的库 Rust 都已经比较齐备。 总体来说，现阶段 Rust 定位的方向还是高性能服务器端程序开发，另外类型系统和语法层面上的创新也使得其可以作为开发 DSL 的利器。
>> [领域专用语言(DSL)](http://www.infoq.com/cn/articles/dsl-discussion)

* TiKV 是一个事务型分布式 Key-Value 数据库，是作为 TiDB 项目的核心存储组件，也是 Google 著名的分布式数据库 Spanner 的开源实现。对于这样一个大型的分布式存储项目，在 TiKV 的开发语言选择上，我们选择了 Rust 语言从零构建。









*舍C++取Rust的思考(http://www.infoq.com/cn/news/2017/09/Select-minority-language-Rust-Ti)*
>认真评估了一下我们的团队背景和要做的东西，最后还是没有选择 C++，原因主要是：
>1. 我们核心团队过去都是 C++ 的重度开发者，也基本都有维护过大型 C++ 项目的经历，每个人都有点心里阴影... 悬挂指针、内存泄漏、Data race 在项目越来越大的过程中几乎很难避免，当然你可以说靠老司机带路，严格 Code Review 和编码规范可以将问题发生的概率降低，但是一旦出现问题，Debug 的成本很高，心智负担很重，而且第三方库不满足规范怎么办。
>>①多个线程对于同一个变量、②同时地、③进行读/写操作的现象并且④至少有一个线程进行写操作。（也就是说，如果所有线程都是只进行读操作，那么将不构成数据争用）(https://blog.csdn.net/gg_18826075157/article/details/72582939)
>2. C++ 的编程**范式太多，而且差异很大，又有很多奇技淫巧**，统一风格同样也需要额外的学习成本，特别是团队的成员在不断的增加，不一定所有人都是 C++ 老司机，特别是大家这么多年了都已经习惯了 GC 的帮助，已经很难回到手动管理内存的时代。
>3. 缺乏包管理，集成构建等现代化的周边工具，虽然这点看上去没那么重要，但是对于一个大型项目这些自动化工具是极其重要的，直接关系到大家的开发效率和项目的迭代的速度。而且 **C++ 的第三方库参差不齐，很多轮子得自己造**。
>
>Rust 在 2015 年底已经发布了 1.0，Rust 有几点特性非常吸引我们：
>- 内存安全性
>- 高性能 (得益于 llvm 的优秀能力，运行时实际上和 C++ 几乎没区别)，与 C/C++ 的包的亲缘性
>>可以简单的把LLVM理解为一个编译器，但是也必须知道，这个编译器可不仅仅是个编译器，它包含了编译相关的各种工具链，并且有一些相对独立的工具，而且它还是开源的。(https://blog.csdn.net/snsn1984/article/details/47039035)
>- 强大的包管理和构建工具 Cargo
>- 更现代的语法
>- 和 C++ 几乎一致的调试调优体验，之前熟悉的工具比如 perf 之类的都可以直接复用
>- FFI，可以无损失的链接和调用 RocksDB 的 C API
>>_FFI_(ForeignFunctionInterface)是用来与其它语言交互的接口
