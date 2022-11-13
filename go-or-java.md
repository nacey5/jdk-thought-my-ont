# go?java?
今天仔细的去看了oracle官方的对于jdk19，已经处于预备阶段的虚拟线程(virtual thread-JEP-425)，我其实大受震撼，因为跟我想的还不太一样。
并且我仔细的思考并且有了以下的，基于我个人的了解，如果有错，欢迎各位指出，谢谢！
在过去的这几年，java的热度慢慢的没有之前那么火了，也包括大厂都在慢慢使用go去过渡掉java，但是，这是之前，具体情况，我不在大厂，
也不好说。但是java一直引以为傲的就是它的jvm以及gc回收一直是处于几乎是最前沿的阶段。但是由于java语言，由于其特性，不支持显示的指针
变量，它的运行速度一直被行业所说，但是！！java因为它的执行引擎使用的是jit+aot的模式，在经过预热之后，其效率甚至不低于c语言这种面对过程
的语言，具体可以到oracle官网查看对比图。
在我了解的过程中，因为在学go语言的时候，我对于go的G-M-P模型非常的佩服，使用协程来接收大量的并发请求，并且通过这个模型可以很好的解决
高并发的问题，因为天生支持高并发的特性(同种语言还有Erlang,这也是rabbit mq的处理为何如此受市场青睐)，这种模型对于传统的只支持使用平台线程
的语言造成了巨大的冲击。因此，由于在过去几年，中国互联网的网民指数型增加，对于大厂来讲，其用户量过于庞大，所以急需一款支持高并发的语言，
`go语言因为其血统（Rob Pike（罗伯.派克），Ken Thompson（肯.汤普森）和Robert Griesemer（罗伯特.格利茨默）`，这几个大名鼎鼎的人物，我就不做
任何介绍了，所以go顺理成章进入中国这个最大的网络市场。
接下来我说虾go的GMP模型：
在操作系统提供的内核线程之上，Go搭建了一个特有的两级线程模型。goroutine机制实现了M : N的线程模型，goroutine机制是协程（coroutine）的一种实现，golang内置的调度器，可以让多核CPU中每个CPU执行一个协程。

![image.png](http://119.23.46.213/static/img/7cd380ff59d4ab8c8f8b39877117b280.image.png)
也就是说，其实go的执行模型，当当前协程无法解决此程序，会将其挂载到令一个处理器中，从而形成一个处理队列，那么也就是说，它的最小的一个单位依然是以一个完整的G为主，可以高并发的处理多个G。当初我在接触到这个模型的时候，被折服了。所以当时我觉得未来的天下应该go会取代java，当时可能是这么想的，现在呢？继续来给大家分享下。
go语言的垃圾回收机制：  混合写屏障+三色标记法。
也就是说，go语言依然没能逃脱掉传统的gc算法不能避免的问题，那就是stw的问题，我不清楚go语言对于goroutine的垃圾回收是怎么样的，因为goroutine不用手动去关闭，当其挂载任务结束后，是直接可覆盖还是何种处理是怎么样的，我不太清楚，这可能需要我再花一些时间继续研究。(目前时间有点紧，见谅，嘿嘿~)
当我看到jdk-19的新特性之后，我有点坐不住，因为之前并没有仔细的好好的去了解[没有经过市场检验的，一般都没怎么去好好看，所以我相信jdk19的很多新特性即使有人去看，恐怕也是都在观望，现在企业开发最多的三个版本为1.7[几乎只存在老版本，但是因为维护等原因，还是有很多老项目使用这套语言维护]]，jdk1.8【对比于7有了很多新特性】，jdk9【模块化管理】。所以jdk19跟现在常用的jdk9都相差了10个大版本。
重点来了！！jdk19出现了一个进入预备阶段的新特性：虚拟线程(virtual thread-JEP-425)
我看完之后，确实大受震撼，震撼的点不仅仅是其融合了go的协程和erlang的进程的处理，也就是jdk19中的virtual thread。
而是我在其中看到了这段话：【因为蹩脚的翻译，所以我直接放上官网的英文，大家看的也比较清晰】

~~~makefile
The stacks of virtual threads are stored in Java's garbage-collected heap as stack chunk objects.
The stacks grow and shrink as the application runs, both to be memory-efficient and to 
accommodate stacks of arbitrary depth(up to the JVM's configured platform thread stack size). This efficiency is what enables a large number of virtual threads, and thus the continued viability of the thread-per-request style in server applications.
In the second example above, recall that a hypothetical framework processes each request by creating a new virtual thread and calling the handle method; even if it calls handle at the end of a deep call stack (after authentication, transactions, etc.), handle itself spawns multiple virtual threads that only perform short-lived tasks. Therefore, for each virtual thread with a deep call stack, there will be multiple virtual threads with shallow call stacks consuming little memory.
~~~
翻译过来就是：
虚拟线程堆栈作为堆栈块对象存储在 Java 的垃圾收集堆中。堆栈随着应用程序的运行而增长和缩小，既是为了节省内存，又是为了适应任意深度的堆栈（直到 JVM 配置的平台线程堆栈大小）。这种效率使大量虚拟线程成为可能，从而使服务器应用程序中的每请求线程样式持续存在。
在上面的第二个例子中，假设一个框架通过创建一个新的虚拟线程并调用该handle方法来处理每个请求；即使它handle在深度调用堆栈的末尾调用（在身份验证、事务等之后），handle它本身也会产生多个只执行短期任务的虚拟线程。因此，对于每个调用堆栈较深的虚拟线程，都会有多个调用堆栈较浅的虚拟线程占用很少的内存。

所以大受震撼，-handle它本身也会产生多个只执行短期任务的虚拟线程-，也就是说，当一个大任务进入到一个虚拟线程进行运行的时候，虚拟线程不仅会自适应堆栈的大小，直至能够一直存在，还能够handler多个执行短期任务的虚拟线程，这就很恐怖了，也就是当前的大任务能够在执行的过程中进行一些拆分，因为我们知道其实虚拟线程是进行挂载到平台线程的，所以也就是说，这个任务的完成处理甚至比我们想象中要快的多[最快为cpu的处理线程有多少条]，并且如果需要其他等待数据的时候，平台线程会进行阻塞，虚拟线程会重新寻求一个平台线程挂载，平台线程的数量甚至比os的线程还能多。
这就很夸张了，java在jdk19融入了这个特性，我个人觉得是有一种想一统天下的气势，但是仅仅是这样子就能一统天下咩？当然不是，我不知道各位是否还技能，oracle手里还把握着一个大杀器，
垃圾回收器的--ZGC！
ZGC真正的做到了传统GC算法中没有办法处理的stw的问题，几乎在任何阶段都是进行并行的，主线程并不会因为标记或者回收或者扫描从而进行停止，我之前也说过了现在的g1算法的一些缺点，zgc
就能够很好的进行改造，并且在jdk15已经进入生产阶段，但是jdk19的默认gc还是g1.
然后，我放上自己画的一张不是很全面的图，如果有错误或者我上述的一些理解有错误，也恳请各位能够帮忙指出，以便我更好的理解，谢谢各位！！

![image.png](http://119.23.46.213/static/img/6b005a6b341d9690258e48d55b0e3852.image.png)
综上，就是我对java的jdk的虚拟线程以及未来的稍微展望，我个人感觉java正在朝着一个方向，那就是进行编程的大一统，但是目前还是几分天下的趋势，毕竟三角定理决定了每种语言如果为了某种优势必须得
舍弃掉其他，所以像python，c，java，go等才会在各自的语言大放异彩[PHP除外，开个玩笑hhhhh].不过我越来越期待，oracle后续会带来什么惊喜！

<!-- more -->
