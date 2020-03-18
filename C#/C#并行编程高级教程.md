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