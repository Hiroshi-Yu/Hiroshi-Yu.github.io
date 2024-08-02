java优化@
===

最近看到一个新内容，转载过来
1. jvm优化—— 垃圾回收
https://my.oschina.net/u/1859679/blog/1548866
2. jvm优化——监控工具
https://my.oschina.net/u/1859679/blog/1552290
3. jvm优化——策略规划
https://my.oschina.net/u/1859679/blog/1556016

以前的内容
---

####先读####
http://hllvm.group.iteye.com/group/topic/27945

####结论####
仅讨论oracle的Java Hotspot和开源的openjdk，由于我只有win环境，linux待测试。
iDempiere的JVM启动只使用了永生代的内存设置，默认192M。
如果报错java.lang.OutOfMemoryError: PermGen space，优化流程如下：



> 内存溢出参数：-XX:+HeapDumpOnOutOfMemoryError

iDempiere服务器的JVM的优化需要考虑：
1. 新生代大小选择
响应时间优先的应用：尽可能设大，直到接近系统的最低响应时间限制（根据实际情况选择）。在此种情况下，新生代收集发生的频率也是最小的。同时，减少到达老生代的对象。
吞吐量优先的应用：尽可能的设置大，可能到达Gbit的程度。因为对响应时间没有要求，垃圾收集可以并行进行，一般适合8CPU以上的应用。

2. 老生代大小选择
响应时间优先的应用：老生代使用并发收集器，所以其大小需要小心设置，一般要考虑并发会话率和会话持续时间等一些参数。如果堆设置小了，可以会造成内存碎片、高回收频率以及应用暂停而使用传统的标记清除方式；如果堆大了，则需要较长的收集时间。最优化的方案，一般需要参考以下数据获得：
  - 并发垃圾收集信息
  - 永生代并发收集次数
  - 传统GC信息
  - 花在新生代和老生代回收上的时间比例
减少新生代和老生代花费的时间，一般会提高应用的效率

3. 吞吐量优先的应用：一般吞吐量优先的应用都有一个很大的新生代和一个较小的老生代。原因是，这样可以尽可能回收掉大部分短期对象，减少中期的对象，而老生代尽存放长期存活对象。

4. 较小堆引起的碎片问题
因为老生代的并发收集器使用标记、清除算法，所以不会对堆进行压缩。当收集器回收时，他会把相邻的空间进行合并，这样可以分配给较大的对象。但是，当堆空间较小时，运行一段时间以后，就会出现“碎片”，如果并发收集器找不到足够的空间，那么并发收集器将会停止，然后使用传统的标记、清除方式进行回收。如果出现“碎片”，可能需要进行如下配置：
  - -XX:+UseCMSCompactAtFullCollection：使用并发收集器时，开启对老生代的压缩。
  - -XX:CMSFullGCsBeforeCompaction=0：上面配置开启的情况下，这里设置多少次Full GC后，对老生代进行压缩

####JVM内存####
JVM规范的内存模型如下图
![JVM内存模型](http://static.oschina.net/uploads/space/2016/0502/203733_LR2V_2720480.jpg)
只讨论常见的：
1. 虚拟机栈（Stacks）也是线程私有的，用于存储局部变量表、操作栈、动态链接、方法出口等信息。
2. Java 堆（Java Heap）是被所有线程共享的一块内存区域，在虚拟机启动时创建。此内存区域的唯一目的就是存放对象实例，几乎所有的对象实例都在这里分配内存。
3. 方法区（Method Area）与Java 堆一样，是各个线程共享的内存区域，它用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。虽然Java 虚拟机规范把**方法区描述为堆的一个逻辑部分（还是在堆中）**，但是它却有一个别名叫做Non-Heap（非堆），目的应该是与Java 堆区分开来。（iDempiere常见内存溢出）
4. 本地区（）

关于Java堆
由于现在收集器基本都是采用的分代收集算法，如果从内存回收的角度看，Java 堆中还可以细分为：新生代和老年代；再细致一点的有Eden 空间、From Survivor 空间、To Survivor 空间等。不过，无论如何划分，都与存放内容无关，无论哪个区域，存储的都仍然是对象实例，进一步划分的目的是为了更好地回收内存，或者更快地分配内存。
 
对于习惯在SUN虚拟机上开发和部署程序的开发者来说，很多人愿意把方法区称为“永生代”（Permanent Generation），本质上两者并不等价，仅仅是因为虚拟机的设计团队选择把GC （垃圾回收器）分代收集扩展至方法区，或者说使用永久代来实现方法区而已。对于其他虚拟机（如BEA JRockit、IBM J9 等）来说是不存在永久代的概念的。

关于内存分配
在Java 语言中，对象访问是如何在Java 栈、Java 堆、方法区这三个最重要内存中进行的呢？如这句代码：
> Object obj = new Object();

假设这句代码出现在方法体中，那“Object obj”这部分的语义将会反映到**Java 栈**的本地变量表中，作为一个reference 类型数据出现。
而“new Object()”这部分的语义将会反映到**Java 堆**中，形成一块存储了Object 类型所有实例数据值（Instance Data，对象中各个实例字段的数据）的结构化内存。
另外，在Java 堆中还必须包含能查找到此对象类型数据（如对象类型、父类、实现的接口、方法等）的地址信息，这些类型数据则存储在**方法区**中。
![JVM对象访问](http://static.oschina.net/uploads/space/2016/0502/205354_IyTT_2720480.jpg)

整个过程如下：
1. JVM会试图为相关Java对象在新生代的Eden空间中初始化一块内存区域
2. 当Eden空间足够时，内存申请结束。否则到下一步
3. JVM试图释放在Eden中所有不活跃的对象（这属于1或更高级的垃圾回收）；释放后若Eden空间仍然不足以放入新对象，则试图将部分Eden中活跃对象放入Survivor区/老生代区
4. Survivor区被用来作为Eden及老生代区的中间交换区域，当老生代区空间足够时，Survivor区的对象会被移到老生代区，否则会被保留在Survivor区
5. 当老生代区空间不够时，JVM会在该区进行完全的垃圾收集（0级）
6. 完全垃圾收集后，若Survivor及老生代区仍然无法存放从Eden复制过来的部分对象，导致JVM无法在Eden区为新对象创建内存区域，则出现”out of memory错误”

####堆内存参数####
-Xms    Java堆内存初始值，默认是物理内存的1/64。
-Xmx    Java堆内存最大值，默认值为物理内存的1/4，最佳设值应该视物理内存大小及计算机内其他内存开销而定。
-Xmn    Java堆新生代大小。永生代一般固定大小为64m，所以增大新生代后，将会减小老生代大小。此值对系统性能影响较大，Sun官方推荐配置为整个堆的3/8。
-Xss      设置每个线程的堆栈大小， 根据应用的线程所需内存大小进行调整。在相同物理内存下，减小这个值能生成更多的线程。

默认空余堆内存小于40%时，JVM就会增大堆直到-Xmx的最大限制；
空余堆内存大于70%时，JVM会减少堆直到-Xms的最小限制。
因此服务器一般设置-Xms、-Xmx 相等以避免在每次GC （内存回收）后调整堆的大小。
说明：如果-Xmx 不指定或者指定偏小，应用可能会导致java.lang.OutOfMemory错误，此错误来自JVM，不是Throwable的，无法用try...catch捕捉。 

####方法区（非堆内存）参数####
-XX:PermSize          
-XX:MaxPermSize   


####回收器参数####
Java中有四种不同的回收算法，对应的启动参数为
–XX:+UseSerialGC
–XX:+UseParallelGC
–XX:+UseParallelOldGC
–XX:+UseConcMarkSweepGC

1. Serial Collector
大部分平台或者强制 java -client 默认会使用这种。
young generation算法 = serial
old generation算法 = serial (mark-sweep-compact)
这种方法的缺点很明显，stop-the-world, 速度慢。服务器应用不推荐使用。

2. Parallel Collector
在linux x64上默认是这种，其他平台要加 java -server 参数才会默认选用这种。
young = parallel，多个thread同时copy
old = mark-sweep-compact = 1
优点：新生代回收更快。因为系统大部分时间做的gc都是新生代的，这样提高了throughput(cpu用于非gc时间)
缺点：当运行在8G/16G server上old generation live object太多时候pause time过长

3. Parallel Compact Collector (ParallelOld)
young = parallel = 2
old = parallel，分成多个独立的单元，如果单元中live object少则回收，多则跳过
优点：old old generation上性能较 parallel 方式有提高
缺点：大部分server系统old generation内存占用会达到60%-80%, 没有那么多理想的单元live object很少方便迅速回收，同时compact方面开销比起parallel并没明显减少。

4. Concurent Mark-Sweep(CMS) Collector
young generation = parallel collector = 2
old = cms
同时不做 compact 操作。
优点：pause time会降低, pause敏感但CPU有空闲的场景需要建议使用策略4.
缺点：cpu占用过多，cpu密集型服务器不适合。另外碎片太多，每个object的存储都要通过链表连续跳n个地方，空间浪费问题也会增大。

####调优工具####


####引用####
http://docs.oracle.com/javase/7/docs/technotes/guides/performance/index.html 官方JVM Hotspot性能调优
http://rainyear.iteye.com/blog/1735121  JVM内存的栈、堆（Heap）、方法区（Non-heap）
http://technique-digest.iteye.com/blog/1123046 JVM的内存参数
http://www.cnblogs.com/edwardlauxh/archive/2010/04/25/1918603.html JVM参数
http://www.blogjava.net/cpegtop/articles/377081.html 前后矛盾较多，仅引用部分

![jmap](http://static.oschina.net/uploads/space/2016/0503/005930_N5si_2720480.png)

