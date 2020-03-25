序
1、高性能软件系统的需求。并行计算技术。
2、处理器的"多核化"。
3、并行计算技术领域分三个自领域：并行计算的理论研究；构建并行计算基础架构；基于并行计算基础架构构建真实软件系统。
4、依据具体应用场景设计或选择一种并行算法，将整个数据处理工作进行切分，然后创建并启动合适数目的线程，并为每个线程分配特定的工作任务。"直接操纵"线程(OS提供的)。
5、串行/并行，保证各个线程之间满足特定的执行顺序需求。确保安全地存取共享资源，并妥善处理系统运行期间可能出现的各种异常。最后，依据系统运行的真实软硬件环境进行性能调优。
6、在更高的抽象层次上开发是提升开发效率的有效途径。.NET 4并行扩展(Parallel Extensions)走的是这条路。
7、.NET 4并行扩展的核心是"任务并行库"(TPL:Task Parallel Library)。它将并行程序的“构造单元”从“线程”(Thread)提升到“任务”(Task)，将任务分派给线程的工作由TPL负责完成，降低并行计算程序的开发难度，提升了开发效率。
8、面向对象系统分析与设计(OOAD:Object Orient Analysis & Design).
9、无线虚拟现实设备和电子玩具。
10、Parallel Programming Talk节目。
11、并行开发困难：申请超额(over subscription)、竞争条件(race condition)、死锁(dead locks)、活锁(live lock)、二步舞(two-step dance)、优先级翻转(priority inversion)、锁封户(lock convoy)、伪共享(false sharing)等。
12、基于任务的编程，到数据并行化、共享状态的管理以及并行程序的调试。
13、Parallel Extensions第一个Community Technology Preview(CTP，2007)。
14、基于任务的编程模型来描述并行。编写运行任务的代码，CLR会自动注入必要的线程。
15、VS2010 带有一个专为并行开发人员设计的IDE。
16、分布式并行化和底层并发编程技术。
17、充分利用任务和延续的简洁性来执行与现有异步编程模型有关的异步任务。
18、“向量化、SIMD指令以及其他并行库”---充分利用现代硬件所提供的其他与并行化相关的能力。函数库，.NET4不直接支持。
19、VS2010 Preminum版或Ultimate版。


附录 A .NET 4 中与并行相关的类图
1、System.Threading.Tasks.Parallel类和结构体，Parallel、ParallelLoopResult、ParallelOptions、ParallelLoopState。
2、Task类、枚举和异常，Task、Task<TResult>、TaskFactory、TaskScheduler、TaskFactory<TResult>、TaskCompletionSource<TResult>;TaskContinuationOptios、TaskCreationOptions、TaskStatus；TaskSchedulerException、UnobservedTaskExceptionEventArgs、OptionCanceledException、TaskCanceledException。
3、并行编程中使用的协调数据结构，并发集合类、轻量级同步原语、延迟初始化类。
4、并发集合类：System.Collections.Concurrent,Partitioner<TSource>、OrderablePartitioner<TSource>、Patitioner、IProducerConsumerCollection<T>；BlockingCollection<T>、ConcurrentBag<T>、ConcurrentDictionary<TKey, TValue>、ConcurrentQueue<T>、ConcurrentStack<T>。
5、轻量级同步原语，System.Threading名称空间下，Barrier、CountDownEvent、ManualResetEventSlim、SemaphoreSlim；结构体，SpinLock、SpinWait。
6、延迟初始化类，System.Lazy<T>、System.Threading.ThreadLocal<T>、System.Threading.LazyInitializer。
7、PLINQ，
8、Threading，Thread、ThradPool、ThreadAbortException、ThreadInterruptedException、ThreadStartExcepion、ThreadStateException。
9、信号相关的类，WaitHandle、EventWaitHandle、AutoResetHandle、ManualResetHandle、Mutex。
10、AbandonedMutexException、CancellationTokenSource、CompressedStack、CountdownEvent、ExecutionContext、HostExevutionContext、HostExecutionContextManager、Interlocked、LockRecursionException、Monitor、Overlapped、ReaderWriterLock、RegisteredWaitHandle、SemaphoreFullException、SynchronizationContext、SynchronizationLockException、Timeout、Timer、WaitHandleCannotBeOpenedException。
11、用于多线程的数据结构、委托和枚举，AsyncFlowControl、CancellationToken、CancellationTokenRegistration、LockCookie、NativeOverlapped。委托，ContextCallback、IOCompletionCallback、ParameterizedCallback、SendOrPostCallback、ThreadStart、TimerCallback、WaitCallback、WaitOrTimerCallback。枚举，ApartmentState、EventRestMode、ThreadPriority、ThreadState。
12、BackgroundWorker组件，


第 1 章 基于任务的程序设计
1、并行化对于发挥现代共享内存多核架构的特性是非常重要的。
2、使用共享内存的多核系统，
3、开发软件时要开始考虑并发性，只有考虑了并发性才能充分发挥微处理器日渐增长的吞吐能力。时钟频率，增加更多的内核。
4、多核架构，软件设计和编码发挥架构的功能很重要。所有的内核都可访问主内存。共享内存的多核架构(shared-memory multicore)，很容易导致性能瓶颈。
5、多核微处理器努力缩减电源消耗，并减少发热量。根据工作负载提升或降低每个内核的时钟频率，甚至可让不在使用中的内核进入睡眠状态。
6、Win7和WinServer2008R2支持一项名为内核暂停(Coore Parking)。
7、现代微处理器可为每个内核使用动态的频率。很难预测指令序列的性能。Intel Turbo Boost Technology能提升活动内核的频率。
8、提升一个内核频率的过程也称为超频(over clocking).
9、微处理器不能让所有的内核长时间超频工作，消耗更多的电力，且温度会迅速上升。所有内核在重负载情况下的平均时钟频率比单个内核能够达到的时钟频率要低。
10、共享内存多核系统与分布式内存系统之间的区别，
11、分布式内存系统，由很多微处理器组成，每个微处理器都有自己私有内存。每个微处理器都可位于不能的计算机上。计算机之间可以有不同的通信信道。消息传递接口(Message Passing Interface,MPI)是运行在分布式内存计算机系统上的并行应用程序所使用的最流行的通信协议。C#可配合MPI来充分发挥共享内存多核系统的特性。
12、MPI主要关注的是帮助开发在集群上运行的应用程序。
13、分布式内存系统会迫使考虑数据分布问题。每一个获取远程数据的消息都会产生一个严重的延迟。
14、并行程序设计和多核程序设计，充分利用底层硬件所提供的并发执行能力。很多种并行架构。
15、顺序指令只能运行在一个可用内核上。
16、现代处理器可在多个数据上执行相同的指令。单指令多数据(Single Instruction,Multiple Data,SIMD)，利用这些向量处理器来减少某些算法的执行时间。
17、共享内训的多核程序设计和向量处理能力的应用。总体目标是减少算法的执行时间。   
18、理解硬件线程和软件线程，
19、物理内核(physical core)---真正独立的处理单元。使用多条指令能够同时并行地运行。运行多个进程，或在在一个进程内运行多个线程。
20、每个物理内核可能会提供多个硬件线程，也称为逻辑内核或逻辑处理器。Intel Hyper-Threading Technology(超线程计技术，HT或HTT)的微处理器在每一个物理内核上提供了多份架构状态。
21、对称多线程(simulaneous multithreading,SMT),它通过额外的架构状态在微处理器的指令级别对并行执行进行优化和增强。
22、注意：一个物理线程并不代表一个物理内核。
23、Windows中每个运行的程序都是一个进程(process)。每一个进程会创建并运行一个或多个线程,软件线程(software thread)。
24、操作系统的调度器在所有要运行的进程和线程之间公平地分享可用的处理资源。Windows调度器会给每一个软件线程分配处理时间。调度器必须从物理内核支持的硬件线程中分配时间给每一个需要运行指令的如那间线程。
25、每一个软件线程与其父进程分享一个私有的唯一的内存空间。但每一个软件线程有自己的栈、寄存器和私有局部存储区域。
26、Windows将每一个硬件线程识别为一个可调度的逻辑处理器(可运行软件线程的代码)。
27、Windows调度器可以决定将一个软件线程赋给另一个硬件线程，均衡每一个硬件线程的工作负载(合理组织有效的资源)。
28、负载均衡(load balancing)指的是将软件线程的任务分发在多个硬件线程的操作。完美实现负载均衡，取决于应用程序的并行程度、工作负载、软件线程数、可用的硬件线程以及负载均衡策略。
29、Windows会给每一个可用的硬件线程分配一块块的处理时间。
30、Core Parking是一项Windows内核电源管理和内核调度技术。增强多核系统上的能源功效。不断跟踪每一个硬件线程相对于其他硬件线程的工作负载，并能决定将某些硬件线程进入睡眠模式。根据负载动态调节正在使用的硬件线程数目。
31、包含HT技术的微处理器系统中，Core Parking会智能地将任务在线程之间进行调度。该策略可降低电源消耗。
32、在调度大量试图并行地运行代码的软件线程的时候，Core Parking可能会引入额外的延迟。在测量并行性能时，一定要考虑可能带来的延迟。
33、理解Amdahl法则，考虑内核数目的变化，并未考虑可以在既有应用程序中添加新功能以充分利用增加的并行处理能力。
34、将代码分解为并行的序列。大部分算法都要求运行一些串行代码来协调并行的执行。以并行的方式启动很多块计算，然后收集它们的计算结果。
35、最大理论性能提升(即加速比，speedup)。最大加速比(倍数) = 1 / ((1- P) + (P/N))。P---表示能够完全并行运行的代码比例。N---表示可用的计算单元数(处理器或物理内核数)。
36、90%的并行效率是非常高的成就了。
37、Gustafson法则，通过问题的大小来测量在固定时间内可执行的工作量。总工作量(单元数) = S + (n x P)。S---表示一次顺序执行完成的工作单元数；P---表示每一部分能够完全并行执行的工作单元数；N---表示可用的执行单元数。
38、使用轻量级并发模型(lightheight concurrency)，减少了在不同逻辑内核上创建和执行代码所需要的总开销。考虑了微架构。
39、将算法分解为多个线程、协调各个代码单元、在代码单元之间共享信息以及收集运算结果等任务实在是非常复杂的程序程序设计工作。
40、多线程编程模型的设计并不是为了帮助开发人员面对多核革命。创建新的线程需要执行大量的处理器指令，且可能会对已经分解为并行化线程的代码引入太大的开销。重量级并发(heavyweight concurrent)。加入了严重的开销，需要编写很多代码来处理由于框架层次缺乏对多线程访问的支持而带来的问题，且导致代码复杂难以理解。针对躲屋里微处理器(只有一个内核)。
41、创建成功的基于任务的设计，理解现有的串行设计，或理解提供了优先可扩展性的并行算法，重构。
42、设计解决方案可采用的技术：将每个问题分解为很多子问题，完全不要去考虑顺序执行。将每个子问题想像为下面三类中的一类，能够以并行的方式进行处理的数据---对数据进行分解；需要很多任务，且能够以某种复杂的并行化进行处理的数据流---对数和任务分解；可以并行运行的任务---任务分解。将设计组织为能够表达并行化的形式。
43、以并行方式进行思考，将要解决的工作分解为任务。通过面向对象的代码对并行化进行封装，创建并行化的对象和组件。
44、以并发的思想指导设计，C#支持并发代码。设计类和方法时，最好将它们设计为没有副作用(side effect)地并发运行。
45、理解交错并发(interleaved concurrency)、并发(concurrency)和并行之间的区别。
46、并发要求物理上能够同时处理才能进行。
47、并行化要求：对需要完成的工作进行划分、并发地运行处理划分的部分、并且能够整合运行结果，对一个问题进行并行化就会产生并发性。
48、并行设计的一大挑战：在各种不同顺序和交错的情况下执行都能够产生正确的结果，即保证正确性(correctness)。
49、并行化任务，为并行化的代码能够实现预期的目标，必须对并行化的代码进行特别的测试和调优。
50、foreach循环比for循环要慢。
51、尽量减少临界区(critical section,在两个并行部分之间需要顺序执行的串行时间段)，串行工作是并行算法整体性能的敌人。对临界区代码进行优化。
52、理解多核并行程序的设计原则，8条原则：按照并行的方式思考；使用抽象编程(TPL)；按照任务(事情)编程，而不是按照线程(CPU内核)编程；设计的时候要考虑关闭并发的情形；避免使用锁；利用为帮助并发而设计的工具和库；使用可扩展的内存分配器；设计大的时候要考虑增长的工作负载而扩展。
53、为最大限度的利用高速缓存，必须分析各种不同的分区情形，尽量避免在每一个任务中耗费大量的内存。
54、Win7和WinServer2008 R2支持最多256个硬件线程或逻辑处理器。
55、为NUMA架构和更改的可扩张性做好准备，
54、多称多处理器(symmetric multiprocessor，SMP)模型，非一致性内存访问(non-uniform memory access,NUMA)体系结构。
55、SMP问题，在于处理器总线称为未来可扩展性的瓶颈，因每一个处理器都能平等地访问内存和I/O。
56、NUMA，每一个处理器访问距离自己近的内存的速度比访问距离自己远的内存的速度要快。
57、在Windows垂直扩展性技术的术语中，MUMA是按照以下方式组织的：一个计算机可以有一个或多个组(group);每个组可以有一个或多个NUMA节点(node)；每个NUMA节点可包含一个或多个物理处理器或插槽(真正的微处理器)；每个处理器或插槽具有一个或多个物理内核；每个物理内核可以提供一个或多个逻辑处理器或硬件线程。
58、NUMA组之间通过共享总线来传输数据。这个过程比访问本地内存的速度要慢。
59、使用NUMA架构的计算机具有不只一个系统总线。每个可用的系统总线都由一组处理器使用。
60、NUMA硬件需要特别的优化。应用程序必须能够感知NUMA硬件及其配置。应用程序必须避免昂贵的内存访问，必须在考虑内存需要的前提下优先使用并发。
61、Win7和WinServer 2008 R2引入了处理器组的概念。在TPC和C#中都不支持这么底层的定义。TPL对NUMA做了优化，能够尽可能地在最合适的内核上运行支持并发的线程，且能够仅可能地使用本地内存。
62、Windows API提供了很多适合NUMA体系结构的函数，但是这些函数并不兼容托管的线程。
63、Coreinfo是一个简单且功能强大的命令行工具，提供有关处理器、值结构和告诉缓存拓扑的非常有用的信息。
64、通过性能测试判断NUMA架构是否会导致性能问题。
65、Coreinfo v2.0，http://technet.microsoft.com/en-us/sysinternals/cc835722.aspx. 它使用Windows的GetLogicalProcessorInformation函数来获取此信息并将其打印到屏幕上。以星号（例如“ *”）表示到逻辑处理器的映射。Coreinfo对于深入了解系统的处理器和缓存拓扑很有用。
66、8MB三级高速缓存，8个硬件线程共享这个缓存。
67、在使用NUMA体系结构时，为避免对外部NUMA节点内存的频繁访问，对不同的分区技术进行测试是一件重要的事情，一切都是为了降低延迟。
68、判断是否适合并行化，一切取决于特定问题的功能需求和性能需求。耗时减少30%。
69、现代硬件体系结构。


第 2 章 命令行式数据并行
1、新的编程模型。
2、数据并行场合使用的新的类、结构和枚举。
3、加载并行任务，TPL是在多核时代应运而生的，能够直接使用的新型轻量级并发模型。
4、TPL支持数据并行(data parallelism)、任务并行(task parallelism)和流水线(pipelining)。它提供了一个轻量级的框架，让开发人员可以应付各种不同的并行场合，实现基于任务的设计，而不用处理重量级且复杂的线程。
5、Advanced Encryption Standard(AES)算法加密。
6、流水线，任务并行和数据并行的结合体。最复杂的情形。对多个并发进行协调。
7、System.Threading.Tasks。主类是Task，表示一个异步的并发的操作。最好创建并行循环或region。
8、Parallel.For---为固定数目的独立的For循环迭代提供了负载均衡的潜在的并发执行。
9、负载均衡的执行会尝试将工作分发在不同的任务中，所有的任务在大部分时间内都可保持繁忙。它总是试图减少任务的闲置时间。
10、Parallel.ForEach---为固定数目的独立For Each循环迭代提供了负载均衡的潜在的并行执行。支持自定义分区器(partitioner)。
11、Parallel.Invoke---对给定的独立任务提供潜在的并行执行。
12、Parallel.Invoke，试图将很多方法并行运行的最简单方式就是使用该方法。安全地并发运行的独立方法。只有在所有方法全部完成执行后才返回，然而即使在执行过程中出现异常，该方法也会完成。一定不能依赖于特定的执行顺序。
13、使用lambda表达式或匿名委托的优势：可定义需要并行运行的复杂的多行方法，而不需要创建额外的方法。
14、没有特定的执行顺序，注意：同样的代码运行的时间不一定是固定的。注意：考虑调度并发任务所需的时间，增加了初始化开销。
15、理解运行并行代码的新型多核硬件是一件非常重要的事情。底层的逻辑根据运行时的现有可用资源创建出最合适的执行计划。
16、Parallel.Invoke优势和权衡，是一种并行运算很多方法的简单方式，不用考虑任务和线程的问题。需要最常的时间才能返回控制(导致很多逻辑内核长时间处于闲置状态)。
17、交错并发，意味着代码中不同的部分可以在交叠的时间段内启动、运行和完成。被定义为虚拟并行化。
18、并发，意味着很多代码可以同时运行。
19、通过时间片机制和快速上下文切换可实现并行执行的假象。
20、实时操作系统(real-time operating system, RTOS)。
21、将串行代码转换为并行代码，找出可并行化的热点(parallelizable hotspot，指代码中需要耗费大量时间)。转换为并行代码、测量加速比、找出潜在的可扩展性，设法确保在将现有的串行代码转换为并行代码时没有引入新的bug。
22、能够检测出串行化代码中的可并行化的热点。
23、检测可并行化的热点，
24、将字符串发送到控制台的操作会产生瓶颈，会影响测量的准确性。
25、测量并行执行的加速效果。加速比 = (串行执行时间) / (并行执行时间)。
26、在数据并行的场合下能够随着可用内核数的增加而获得更好的可扩展性。
27、使用TPL的类和方法的时候，不需要对TPL记性初始化。TPL在后台执行了大量工作，尽其所能地优化调度机制，充分利用底层硬件。选择正确的结构对热点进行并行化是一项非常重要的任务。
28、理解并发执行，
29、循环并行化，易并行计算问题(embarrassingly parallel).
30、Parallel.For，采用适合这个新方法的参数。不支持浮点数和步进。循环体是并行运行的，迭代范围的分区是根据可用的硬件资源进行的。无法预测执行顺序。可返回一个Parallel.LoopResult值。
31、同步结构(synchronization structure)，保护变化的值和状态。同步开销
32、创建安全无状态的代码。
33、不同的硬件配置会使得串行代码和并行代码都成生不同的结果。
34、循环进行大量的内存分配操作,默认的工作站垃圾回收(garbage collection, GC可能会降低可扩展性;启动服务器GC。
35、并行化的代码能够随着内核数的增长而扩展，这对于Parallel.Invoke版本来说是不可能的。可扩展性都有一定的局限性。并行化的代码使用的内核数达到一定数目的时候，并行算法就不能获得额外的加速了。
36、对委托的调用比直接调用方法的开销要大。在并行迭代中的分配和协调也会带来额外的开销。
37、测试得到加速比，确定性能提升。
38、易并行的计算问题通常不需要复杂的协调机制。
39、Parallel.ForEach,用于处理一组数据的通用机制。利用一个范围的证书作为一组整数，然后通过一个自定义的分区器将这个范围转换为一组数据块。每一块数据都通过循环的方式进行处理，这些循环是并行执行的。
40、将所有的要处理的数据分区为多个部分，使这些部分在较小的并行运行的独立串行循环中运行。搭配自定义的分区器(custom partitioner),创建一些列新版本的串行循环，且这种重构更加简单。
41、Partition.Create使用的是内置的默认值。根据逻辑内核数、分区大小以及其他因素所决定的。
42、根据内核的数目优化分区，System.Environment.ProcessorCount提供了逻辑内核的数目。
43、对算法的工作进行分析，并且考虑现代硬件架构所能够提供的处理能力。降低因为首次并行化而引入的不必要的开销。
44、使用带有IEnumerable接口的数据源，Enumerable实际上是串行遍历的。Parallel.ForEach加载的所有支持任务执行的线程在访问迭代器的时候都被迫串行化。避免，可创建一个能够进行静态范围分区的自定义分区器。
45、从并行循环中退出。
46、ParallelLoopState的loopState实例提供了以下两个方法用于停止Parallel.For和Parallel.ForEach的执行，Break---告诉并行循环应该在执行了当前迭代之后尽快停止执行；Stop---告诉并行循环应该尽快停止执行。
47、判断是否有请求要求结束并行循环的执行，可检查ShouldExitConcurrentIteration只读属性。
48、取消一个并行循环的请求也可以是外部的，即以cancellation的形式出现。
49、分析并行循环执行的结果，LowestBreakIteration属性的值为调用Break语句的值最小的那个迭代的值。
50、如果并行循环没有因为抛出异常而终止，说明所有迭代都完成了。
51、捕获并行循环中发生的异常。并行化迭代中调用的委托中的代码抛出异常，这个异常没有在委托中捕获到时，这个异常会变成一组异常。System.AggregateException类负责处理这一组异常。
52、InnerExceptions是Exception的只读集合。
53、需要更为先进的异常管理技术。
54、指定并行度，最大并行度(maximum degree of parallelism).TPL的方法总会视图利用所有可用的逻辑内核以追求实现做好的结果。ParallelOptions，这个实例的MaxDegreeOfParallelism属性的值。
55、利用所有的内核(MaxDegreeOfParallelism = -1).未设置这个属性，TPL就会允许通过启发式算法提高或降低线程的数目，通常都会高于ProcessorCount，可更好地支持CPU和I/O混合型的工作负载。
56、ParallelOptions，控制高级选项，CancellationToken，传播尖并行操纵的通知。TaskScheduler，通常不需要定义一个自定义的任务调度器来调度并行任务，除非并行任务使用了非常特殊的算法。
57、计算硬件线程，Environment.ProcessorCount逻辑内核的数目，有时逻辑内核的数目和物理内核的数目是不一样的。
58、线程是多部分代码并行运行的底层线裤路(lane),能够充分利用底层硬件所提供的多个内核。
59、代码可能会等待从微处理器或系统内存中不同的高速缓存中获取数据，会产生延迟。存在闲置的执行单元。
60、HTT提供了栓分架构状态，提供了增强的指令级并行。
61、逻辑内核并不是物理内核，必须对底层的硬件非常了解，才能测试可扩展性，避免得出错误的结论。
62、通过甘特图(Gantt chart)检测临界区，甘特图是一种线条图，用于图解项目安排以及相关联任务之间的依赖关系。理解任务之间的依赖关系，且判断哪些任务可以并行运行，哪些不能并行运行。
63、好的设计对于应用程序的性能和编码而言总是非常有帮助。

第 3 章 命令式任务并行
1、创建并管理多个线程和使用线程池充分利用多核技术和多处理器。新的Task实例可通过更简单的代码解决命令式任务并行问题和编写复杂的算法。
2、使用任务而不是线程来创建并行代码，且描述了与每一个场景相关的一些新概念。基于任务的编程模型。
3、创建和管理任务，
4、TPL基于任务的编程模型，可发挥多核的功效，提升应用程序的性能，且不需要编写底层、复杂且重量级的线程代码。任务的运行需要使用线程。任务和线程并没有一对一的关系。
5、运行在多个线程中的同步代码是非常复杂的。TPL，将一些同步问题(特别是工作调度机制)隐藏在身后。
6、CLR通过工作窃取队列(work-stealing queue)减少锁的使用，以及调度小的工作块(work chunk),而不会增加明显的开销。
7、创建新的线程会引入巨大的开销，但创建新的任务能够从现有的线程中窃取工作。
8、默认的任务调度器依赖于底层的线程池引擎。调度器会使用工作窃取队列找到一个最合适的线程，然后将任务加入队列。
9、System.Threading.Tasks.Task，
10、Parallel.Invoke,TPL会在幕后，创建Task实例，创建与调用委托数目一致的实例。
11、一个Task表示一个异步操作(asynchronous operation)，对执行进行控制，且获得关于其状态的信息。Task的创建和其执行是独立的，因此可对相关联操作的执行拥有完全的控制权。
12、Task只读属性，AsyncState(状态对象)，CreationOptions，CurrentId(正在执行的Task的唯一ID，不同于非托管代码中的ID)，Exception，Factory，Id，IsCanceled，IsCompleted，IsFaulted，Satus
13、理解Task状态和生命周期，一个Task实例只会完成其生命周期一次。当Task到达它的3种可能的最终状态之一时，它就再也回不去之前的任何状态了。
14、一个Task表示并发的代码，这些代码的执行取决于底层硬件和运行时可用的资源。
15、TaskStatus，初始状态，三种：Created(Task构造函数创建，Start或RunSynchronously方法改变，取消)，WaitingForActivation(使用定义延续的方法创建的)，WaitingToRun(TaskFactory.StarNew，等待某个特定的调度器挑选自己并运行)。
16、TaskStatus，最终状态，running，WaitingForChildrenToComplete(关联子任务)。Canceled，Faulted(存在未处理的异常，导致任务结束)。RanToCompletion。
17、通过使用任务来对代码进行并行化。使用Parallel Tasks和Parallel Stacks窗口对任务进行可视化。
18、任务通过一个是自动生成的匿名方法名称和一个数字识别的。
19、线程的分配取决于当前可用的硬件资源以及CLR任务调度器和Windows调度器所采取的操作。
20、CLR任务调度器会试图从最合适的线程中窃取工作。还会决定创建新的线程支持任务的执行。并不能保证底层的线程能够并行运行。操作系统调度器会在数十个甚至数百个被调度在可用内核上使用处理器时间的线程之间分配内核。
21、工作线程必须使用委托来更新UI，在主线程中运行代码。在基于任务的编程模型，UI的更新是一个很复杂的主题。
22、等待任务完成，等待任务完成花费的时间使用的是轻量级的机制，能够尽可能地节省CPU周期。
23、忘记复杂的线程，Task.WaitAll方法在超时的时候并不会取消任务---只是结束同步执行并返回一个bool结果。
24、Task实例的Wait方法。可接收超时时间。
25、通过取消标记取消任务，cancellation token，中断Task实例的执行。委托中添加一些代码。一个CancellationToken实例，且通过调用ThrowIfCancellationRequested方法抛出OperationCanceledException异常。
26、任务的实现将会在调研委托之前检查取消标记。
27、CancellationTokenSource能够初始化取消的请求。CancellationToken能够将这些请求传递给异步的操作。
28、。CancellationToken必须作为参数传递给每一个任务委托。它的实例通过信号机制来传播必须取消的操作。
29、TaskFactory，更多的选项。异步操作的取消非常简单，只需添加几行代码就可以了。添加必要的清理代码是非常重要的。
30、处理任务抛出的异常，
31、从任务返回值，为调用函数并且使用Task<TResult>实例。
32、TaskCreationOptions，定义任务创建、调度和执行的可选行为。
33、通过延续(continuation))串联多个任务，任务实例上调用ContinueWith方法。
34、链式任务(chained tasks)。
35、通过延续混合并行代码和并行代码，使用复杂的延续。
36、TaskContinuationOptions参数，带有一些标志，可以控制延续另一个任务的任务调度和执行的可选行为。
37、通过任务编写复杂的带有临界区的并行算法。延续时，向调度器提供的信息非常有价值，调度器通过这些与任务有关的信息能够判断当前的任务完成之后应该如何分配新的任务。优秀的设计能够避免额外的开销。
38、编写适应并发和并发的代码，新的数据结构，简化很多复杂的同步问题：并发集合类、轻量级同步原语、惰性初始化的类型。尽可能避免使用锁(lock)，且在必要的情况下使用的是细粒度的锁。锁会产生很多潜在的bug，且可能会极大地降低可扩展性。


第 4 章 并发集合
1、5个新类和一个新接口。
2、理解并发集合提供的功能，并发集合已经准备好了接受并发和并行的方法调用，解决潜在的死锁问题和竞争条件的问题。尽可能地减少了需要使用锁的次数，使得在大部分情形下能够优化为最优性能。
3、死锁和竞争条件的问题可能很难侦测。
4、线程安全并不是没有代价的。并发集合会有更大的开销。只需从多个任务中并发访问集合的时候使用并发集合。
5、System.Collections.Concurrent,解决线程安全的问题。BlockingCollection<T>，适用于有多个任务添加和删除数据的生产者--消费者的情形，是对一个IProducerConsumer<T>实例的包装器，提供了阻塞和限界的能力；ConcurrentBag<T>，提供了一个无序的对象集合；ConcurrentDictionary<TKey,TValue>；ConcurrentQueue<T>；ConcurrentStack<T>。在某种程度上使用了无锁技术，可获得性能提升。这些集合通过使用比较并交换(compare-and-swap，CAS)指令和内存屏障(memory barrier)，避免了使用典型的互斥的重量级锁。
6、ConcurrentQueue，完全无锁的。永远不会获得锁，但是当CAS操作失败且面临资源争用，可能会自旋并且充实操作。
7、资源争用，指的是当很多任务或线程试图同时使用一个资源时所发生的情况。
8、理解并行的生产者-消费者模式(producer-consumer)，分解两阶段的线性流水线。
9、通过信号机制，可避免使用低效的循环。不同的任务和线程之间进行通信管理。
10、使用多生产者和消费者。多阶段的线程流水线。模式的整体性能和引入的延迟测量是非常重要的事情。
11、ConcurrentQueue，必须维护一个FIFO的顺序。
12、通过并发集合设计流水线，
13、一个完美的线性流水线中段的数目应该与可用的逻辑内核数目相等，且每一个阶段都应该有等量的工作。
14、ConcurrentStack，重要方法，Push、Trypop、TryPeek。IsEmpty，判断栈是否包含任意项。比ConcurrentQueue速度快。在一个原子操作内将多个元素插入栈顶或从栈顶移出，PushRange和TryPopRange。Clear方法移除所有项(原子操作)。
15、将使用数组和不安全集合的代码转换为使用并发集合的代码，创建一个新的并发集合实例，将不安全的集合作为参数传入。
16、使用以下方法根据并发队列中的元素创建一个不安全的集合，CopyTo，ToArray。
17、ConcurrentBag，在同一个线程添加元素(生产)和删除元素(消费)的场合效率特别高。使用很多不同的机制，最大程度地减少了同步的需求以及同步锁带来的开销。有时会需要锁(完全分开的场景，效率非常低)。
18、ConcurrentBag为每一个访问集合的线程维护了一个本地队列，会以无锁的访问这个本地队列。
19、ConcurrentBag表示一个无序组(bag)，即一个无序的对象集合，且支持对象重复。方法：Add、TryTake、TryPeek。IsEmpty。
20、检查ConcurrentBag的IsEmpty属性的值的开销非常大，需要临时获取这个无序组的所有锁。
21、volatile关键字。可确保在不同的线程中进行访问的时候，可得到最新值。
22、IProducerConsumerCollection，接口定义了对用于生产者-消费者情形的并发集合进行操作的方法。从ICollection、IEnumerable和IEnumerable<T>继承而来。CopyTo、ToArray、TryAdd、TryTake。
23、BlockCollection，对IProducerConsumerCollection<T>实例的一个包装。支持限界(bound)和阻塞(block)。对生产者-消费者场景和流水线的实现都非常有用。
24、BoundedCapacity属性保存了为集合指定的最大容量。当集合容量达到这个值时，添加元素的请求，生产者任务或线程将会被阻塞。限界功能对于控制内存中集合的最大大小，特别是处理大量元素的时候，非常有用。
25、当IsAddingCompleted为true且集合为空时，IsCompleted属性将会被设置为true。集合已经被生产者标记为完成。
26、GetConsumingEnumerable方法返回底层集合的枚举器，且允许使用foreach移除元素。阻塞循环，等待添加新的元素。更高效。
27、应该避免使用不必要的自旋。
28、默认情况，BlockingCollection封装了一个ConcurrentQueue。可在构造函数中指定一个不同类型的病啊集合。元素的顺序将会发生改变。
29、取消BlockingCollection进行的操作。
30、使用并发集合的错误实现可能会导致任务永远等待。
31、通过多个BlockingCollection实例实现一个过滤流水线。AddToAny，TryAddToAny；TakeFromAny和TryTakeFromAny。
32、随机数生成器并不会太高效。
33、ConcurrentDictionary，对读操作是完全无锁的。对频繁使用读取操作的场景进行了优化。添加或修改，使用细粒度的锁。AddOrUpdate，键不存在，添加新键值对，存在，更新(不是线程安全)。GetEnumerator；GetOrAdd；TryAdd；TryGetValue；TryRemove；TryUpdate。
34、IEqualityComparer接口。


第 5 章 协调数据结构
1、不同并发任务所执行的工作的同步。经典的同步原语。轻量级协调数据结构。
2、通过汽车和车道理解并发难题，
3、副作用(side effect)。
4、竞争条件，很多并发任务在没有提供合适的同步机制的情况下对同一个变量进行读写时，可能会发生。这是一个正确性的问题。
5、死锁(deadlock)，
6、使用原子操作的无锁算法，
7、使用本地存储的无锁算法，将数据分区，对每一个任务使用本地存储。要求更多的内存。限制可扩展性。
8、理解新的同步机制，并发集合类、轻量级的同步原语、延迟初始化(lazy Initialization)的类型。
9、尝试以隐式同步的方式(也会增加开销)对问题进行分解。显式的同步、原子操作和锁总是会增加额外的开销，需要更多的处理器时间，且降低可扩展性。
10、使用同步原语，6个：Barrier，允许多个任务同步它们不同阶段上的并发工作；CountdownEvent，简化fork和join的情形；ManualResetSlim，允许很多任务等待直到另一个任务手工发出事件句柄；SemaphoreSlim，允许限制能够并发访问一个资源或一个资源池的任何的数目；SpinLock，允许一个任务自旋直到获得一个互斥锁，它是一个struct，若需要大量的锁且最小化对象的分配；。
11、在循环中等待的过程称为自旋。忙等(busy waiting)；SpinWait，允许一个任务执行基于自旋的等待，直到指定的条件的得到满足。
12、自旋然后等待(spinning-then-waiting)。二阶段等待操作(two-phase wait operation)。自旋等待的时间超过一定值且条件没有得到满足，就进入内核的等待(kernel-based wait)。
13、通过选择合适的同步原语来降低开销是一件非常重要的事情。
14、VS2010提供了新的并发剖析功能。对比测试经典的Monitor锁和关闭了调试功能的SpinLock锁的性能。
15、通过屏障同步并发任务，组中的每一个任务都称为一个参与者(participant)---在每一个阶段到达Barrier时都会发出信号表示自己已经到达，且隐式地等待所有其他参与者发出信号之后才会继续执行下去。
16、每当屏障接收到来自所有参与者的信号之后，屏障就会递增其阶段数，运行构造函数中指定的动作，且解除阻塞每一个参与者的Task实例。
17、SignalAndWait方法，发出信号，表示参与者已经到达屏障，且开始等待所有其他参与者也到达屏障。
18、屏障和ContinueWhenAll，每次使用完屏障之后，总是应该将屏障进行处理。
19、两三个阶段运行一组任务且只运行一次，使用ContinueWhenAll方法会比使用屏障更为方便。在一个循环内运行很多阶段，那么屏障通常可以提供做好的方案。
20、在所有的参与者任务中捕获异常，包装在BarrierPostPhaseException中，而且所有的参与者任务或线程都能捕获到这个异常。
21、抛出异常的开销很大。
22、使用超时，对于一个超时对于防止任务或线程的无限阻塞来说是非常重要的。
23、Parallel Tasks窗口，显示的状态可能是Waiting或Running，取决于是否捕捉到了正在睡眠或等待状态中的自旋状态。
24、使用动态数目的参与者。
25、使用互斥锁(mutual-exclusion lock)，lock关键字(Monitor.Enter和Monitor.Exit)。私有对象具有排它访问权。锁定引用类型或一个独立的同步对象。
26、避免锁定外部对象，避免跨成员或类的边界获得和释放一个锁。临界区中的代码应尽量保持简单。
27、使用锁超时，持有一个锁时，应避免阻塞或调用任何可能导致其阻塞的代码。Monitor.TryEnter方法，检测bool变量的值。
28、应尽量使用超时来避免复杂的难以检测的死锁问题。
29、Monitor类的3个附加方法，Pulse、PulseAll、Wait。
30、将代码重构为避免使用锁，使用剖析功能帮助选择最方便的方案。
31、将自旋锁用作互斥锁原语。持有锁的时间总是非常短，且锁的粒度很精细。使用SpinLock最佳性能，false值，禁用作为调试目的而使用的跟踪线程ID的选项。阻塞，在一个循环内等待，且不断地检查锁是否变得可用。没有使用内存屏障，所以可以通过牺牲功能平性来获得性能提升。注意：不能将SpinLock声明为只读字段，会导致每次调用这个字段都会返回SpinLock的一个新的副本，而不是原始的那个。
32、使用基于自旋的等待，等待的某个条件满足需要的时间很短，且不希望发生昂贵的上下文切换。SpinWait，基本的自旋，且SpinUntil方法。不会产生不必要的内存分配的开销。
33、长时间的自旋并不是很好的做法。自旋会阻塞更高优先级的线程及其相关的任务，还会阻塞垃圾回收器。
34、自旋时间过长，SpinWait会让让出底层线程的时间片，并触发上下文切换。
35、SpinLock只不过是对SpinWait的简单包装。
36、一个线程自旋时，会将一个内核放入到一个繁忙的循环中。
37、一个任务或线程调用Thread.Sleep方法，底层的线程会让出当前处理器时间片的剩余部分，这是一个很大开销的操作。大部分情况下，不要在循环内部调用Thread.Sleep方法。给Thread.Sleep传0，当前线程会挂起，允许其他等待的线程开始执行。
38、自旋和处理器出让，Reset，重置自旋计数器；SpinOnce，执行一次自旋。Count；NextSpinWillYield，表示下一次通过SpinOnce方法进行自旋是否会出让底层线程的时间片且触发一次上下文切换。
39、SpinWait后退式的自旋逻辑，不应该在多个线程中使用SpinWait。
40、自旋和处理器出让取决于CPU的处理能力。自旋在单核计算机上没有什么实际的作用。
41、信号的开销比自旋的开销大。出让处理器的操作开销很大，没必要一直自旋并且出让处理器。
42、使用volatile修饰符，避免编译器因为假定这个字段只会被一个线程访问而做的优化。保证这个：当这个共享变量被不同的线程访问和更新且没有锁和原子操作的时候，最新的值总能在共享变量中表示出来。
43、可将volatile修饰符应用于类和struct的字段，但不能声明使用volatile的局部变量。
44、使用轻量级的手动重置事件。手动重置事件是一个带有两个可能状态的时间对象：设置/发出信号(true)和取消设置/取消信号(false)。从一个任务向另一个任务发出信号，表明发生了一个事件。
45、预计等待的时间非常短，使用轻量级ManualResetEventSlim可以发出信号并等待事件句柄(event handle).
46、使用ManualResetEventSlim进行自旋等待和基于内核的等待的组合。也更适合于任务的编程模型。
47、信号机制是在底层线程级别产生作用。Wait函数会阻塞当前任务和底层的线程，知道事件对象收到信号或达到指定的超时时间为止。
48、使用ManualResetEventSlim进行自旋和等待，它通过封装了手动重置事件的事件等待句柄提供了自旋等待和基于内核等待的组合。使用这个类的实例在任务之间发送信号，且等待事件的发生。Reset，设置为false，Set设置为true，Wait阻塞当前任务或线程，直到另一个任务或线程调用这个实例的Set方法。属性，IsSet，SpinCount，WaitHandle提供了封装操作系统对象的WaitHandle对象，通过这个对象可以等待对共享资源的排它访问。
49、当使用完ManualResetEventSlim实例的时候，一定要将其抛弃。
50、使用超时时间和取消，
51、与调用Parallel.Invoke相比，在发生问题时，直接创建的Task实例能够更好地控制取消标记在Task实例中的传播。
52、使用ManualResetEvent，实现跨越进程或跨AppDomain的同步。没有IsSet，也不能使用取消标记。WaiOne方法总是执行基于内核的等待。公开了一个SafeWaitHandle属性，通过这个属性可获得或设置原生的操作系统句柄。
53、限制资源的并发访问，表示计数信号量(counting semaphore)的System.Threading.Semaphore类非常有用。表示一个Windows内核信号量对象。SemaphoreSlim，跟适合对任务进行处理，没有使用Windows内核的信号量对象。
54、计数信号量，通过跟踪进入和离开的任务或线程来协调对资源的访问。信号量需要知道能够通过信号量协调机制所访问共享资源的最大任务数。
55、信号量会降低可扩展性，且信号量的目的就是如此。必须非常小心地只有在必要的时候才使用信号量。
56、使用SemaphoreSlim，控制对共享资源的访问。AvailableWaitHandle，提供了封装操作系统对象的WaitHandle对象，可以等待信号量，但是不会改变信号量的计数。
57、能够在并发任务中生成真正的随机数，Paralle Extension Extras提供了关于ThreadSafeRandom类。
58、使用Semaphore，不能使用取消，没有用于获取能够获准进入信号量的CurrentCount属性。
59、通过CountdownEvent简化动态fork和join场景。
60、需要对数目随着时间变化的任务进行跟踪。CountdownEvent是一个非常轻量级的同步原语。与Task.WaitALL或Task.Factory.ContinueWhenAll等比，开销要小的多。Wait，直到该实例被设置(即信号计数已经为0)；Signal，注册一个信号，且将sheng。
61、典型的fork/join场景，每当一个任务完成工作的时候，这个任务都会发出一个CountdownEvent实例的信号，并将其信号计数递减1。
62、使用原子操作，System.Threading.Interlocked类，在共享变量上执行单次操作(要么完成，要么失败)。开销非常低，非常高效，且线程安全。底层：将共享变量的值加载到寄存器中；增加寄存器的值；将新的值存储在共享实例变量中。
63、使用一个独立的同步对象是一种常见的做法。
64、正确性比好的性能要重要得多。


第 6 章 PLINQ：声明式数据并行
1、declarative data parallelism。PLINQ，混合使用任务和数据分解。
2、理解潜在的性能瓶颈以及解决这些瓶颈的不同技术也是非常重要的事情。
3、从LINQ转换到PLINQ，具体加速效果取决于具体的场景。
4、ParallelEnumerable及其AsParallel方法，
5、AsOredered和Orderby子句。
6、要求顺序结果的PLINQ查询并不能获得明显的性能提升。另一种方法：在多个独立的任务中，或者使用Parallel.Invoke运行很多个LINQ查询。
7、指定执行模式，PLINQ是使用支持并行执行的任务和线程来并行执行查询中的不同部分。.NET会根据查询的形态来做出决策，而并不考虑数据集额大小和委托的执行时间。强制并行执行，WithExecutionMode方法并传入ParallelExecutionMode.ForceParallelism来实现。
8、理解PLINQ中的数据分区，复杂的查询在执行过程中可能还会重新分区。接受输入数据源之后，根据可用逻辑内核数将数据分解为多份。然后使任务在不同的内核上处理每一部分。
9、根据查询形态的不同，PLINQ执行引擎会使用以下4个主要的算法来进行数据分区：范围分区(range partitioning),可用于可索引的数据源(如列表和数组，通过IList接口查询)；数据块分区(chunk partitioning),可用于任何数据源；交错式分区(striped partitioning),对在数据源顶部对数据项进行处理的情况进行了优化，SkipWhile或TakeWhile；散列分区(hash partitioning)，对数据元素的比较进行了优化，数据和任务之间建立了通道，带有相同散列码的数据项会被发送到同一个任务中(distinct,except,groupjoin,groupby,intersect,join,union)。
10、通过PLINQ执行规约操作，PLINQ可简化对一个序列或一个组中的所有成员因公一个函数的过程，这个过程称为规约操作。Average、Max、Min、Sum。一定要通过对应的串行版本来定义原始的数据源，帮助PLINQ获得最优的执行结果。
11、创建自定义的PLINQ聚合函数，提供一个重载的Aggregate，允许实现自定义的并行规约算法。
12、标准偏差(Standard deviation)---所有值从平均值偏离的程度。
13、偏度(Skewness)---一个分布在其平均数周围的不对称程度。
14、峰度(Kurtosis)---分布相对于正态分布而言是更加高耸还是更加平坦。
15、Aggregate使用4个参数计算标准偏差，累加器函数的初始值；更新累加器函数；合并累加器函数；结果选择器。
16、将所有的操作都放在一个Aggregate调用中的效率可能比对每一个操作都做一次Aggregate调用多次效率更高。
17、并发PLINQ任务，
18、取消PLINQ，
19、指定所需的并行度，WithDegreeOfParallelism。
20、Environment.ProcessorCount，需要Plarform Invoke(P/Invoke)。要求进行一次平台调用，这个调用的开销并不小。
21、测量可扩展性，
22、使用ForAll，接受一个动作(Action)作为参数。
23、foreach和ForAll的区别，
24、当超过物理内核，额外的任务需要在同一个物理内核的超线程内核上运行。
25、通过WithMergeOptions配置返回结果的方式，AutoBuffered,使用一个输出缓冲来基类结果；Default，使用AutoBuffered；NotBuffered，增加同步开销，增加整体的执行时间，降低获得第一个结果元素的潜伏时间；FullyBufferred，使用完整的输出缓冲来基类结果，减少同步开销。
26、处理PLINQ抛出的异常，
27、PLINQ查询有延缓执行的效果。一定要捕捉查询所产生的结果数据在被消费者消费时产生的异常。
28、使用PLINQ执行MapReduce算法，
29、MapReduce算法，是一种非常流行的算法框架，能够充分利用并行化处理巨大的数据集。基本思想：将数据处理问题分解为以下两个独立且可以并行执行的操作：映射(Map)，对数据源进行操作，为每一个数据项计算出一个键值对;规约(Reduce)，对映射操作产生的根据键进行分组的所有键值对进行操作，对每一个组执行规约操作。
30、工作节点，work node。
31、使用IGrouping从一个集合中将带有同一个键的元素挑选出来。
32、使用PLINQ设计串行多步操作。
33、定位处理的瓶颈。