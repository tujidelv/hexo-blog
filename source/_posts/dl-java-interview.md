---
title: Java常见面试题
date: 2019-08-18 16:46:58
categories:
- 开发语言
tags:
- java
---

# Java常见面试题

## 目录

- [简介](#简介)
- [正篇](#正篇)
- [参考链接](#参考链接)
- [结束语](#结束语)

## 简介

记录自己面试时遇到的一些面试题。

## 正篇

### 家娃基础专题

- 对Java平台的理解
    ```
    Java是一种面向对象的编程语言，最显著的特性有两个方面
        一个是“书写一次，到处运行”（Write once, run anywhere），能够非常容易地获得跨平台能力；
        另一个是垃圾收集（GC, Garbage Collection），Java 通过垃圾收集器（Garbage Collector）回收内存，大部分情况下，程序员不需要自己操心内存的分配和回收。
    对于“Java是解释执行”这句话，这个说法不太准确
        我们开发的Java 的源代码，首先通过Javac编译成为字节码，然后在运行时，JVM 会通过类加载器（Class-Loader）加载字节码，并通过内嵌的解释器将其转换成为最终的机器码。
        但是常见的JVM，比如我们大多数情况使用的 Oracle JDK 提供的 Hotspot JVM，都提供了JIT（Just-In-Time）编译器，也就是通常所说的动态编译器，
        它能够在运行时将热点代码编译成机器码，这种情况下部分热点代码就属于编译执行，而不是解释执行了。
     在JDK 8 实际是解释和编译混合的一种模式，即所谓的混合模式（-Xmixed）
        Oracle Hotspot JVM 内置了两个不同的 JIT compiler，C1对应前面说的client模式，适用于对于启动速度敏感的应用，比如普通 Java 桌面应用；
        C2对应server模式，它的优化是为长时间运行的服务器端应用设计的。默认是采用所谓的分层编译（TieredCompilation）。
    ```

- Exception、Error的区别
    ```
    Exception和Error都继承自Throwable类
        只有Throwable类型的实例才可以被抛出（throw）或者捕获（catch），它是异常处理机制的基本组成类型。
        Exception和Error体现了Java平台设计者对不同异常情况的分类。
    Error是指在正常情况下，不大可能出现的情况，绝大部分的Error都会导致程序（比如 JVM 自身）处于非正常的、不可恢复状态。
        既然是非正常情况，所以不便于也不需要捕获，常见的比如 OutOfMemoryError、StackOverflowError等都是Error的子类。
    Exception是程序正常运行中，可以预料的意外情况，对于这类异常，应该尽可能处理异常，使程序恢复，分为检查型（checked）异常和非检查型（unchecked）异常。
        检查型异常：在源代码里必须显式地进行捕获处理，否则编译不通过;非RuntimeException都是此类异常.
            IOException、SQLException、ClassNotFoundException等
        非检查型异常：在源代码里可以不处理(可以修改代码更严谨),也可以处理;RuntimeException及其子类都是此类异常,Error类也可归为此类.
            NullPointerException、IndexOutOfBoundsException、ClassCastException、IllegalArgumentException等
    ```
- ClassNotFoundException与NoClassDefFoundError区别
    ```
    前者的根本原因是.class文件找不到,例如少引了某个jar。
        解决方法是通常需要检查一下classpath下能不能找到包含缺失.class文件的jar。
    后者是类加载器试图加载类的定义，但是找不到这个类的定义，而实际上这个.class文件是存在的。
        解决方法是需要检查这个类定义中的初始化部分（如类属性定义、static 块等）的代码是否有抛异常的可能，如果是 static 块，可以考虑在其中
        将异常捕获并打印堆栈等，或者直接在对类进行初始化调用（如 new Foobar()）时作 try  catch。
    ```
- 异常处理原则
    ```
    尽量不要捕获通用异常,而应该捕获特定异常
        这样能让自己的代码能够直观地体现出尽量多的信息,方便快速定位问题。
    不要生吞异常,即捕获后不要什么都不做
        这样程序可能在后续代码以不可控的方式结束,没人能够轻易判断究竟是哪里抛出了异常，以及是什么原因产生了异常。
    只捕获有必要的代码段,尽量不要一个大的try包住整段的代码
        try-catch代码段会产生额外的性能开销，它往往会影响JVM对代码进行优化。
    不要使用异常处理块控制代码流程
        Java每实例化一个Exception，都会对当时的栈进行快照，这是一个相对比较重的操作。如果发生的非常频繁，这个开销可就不能被忽略了。
    ```
- throw、throws的区别
    ```
    throw：
        在方法体中,后面跟的是异常对象,并且只能是一个
        throw抛出的是一个异常对象,说明这里肯定有一个异常产生了
    throws：
        在方法声明上,后面跟的是异常的类名,可以是多个
        throws是声明方法有异常,是一种可能性,这个异常并不一定会产生    
    ```
- final、finally、finalize的区别
    ```
    final：最终的意思,可以修饰类,变量,方法
        修饰类,该类不能被继承;修饰变量,该变量常量,不能被重新赋值;修饰方法,该方法不能被重写
    finally：是异常处理的一种机制,用来保护重点代码一定要被执行,例如关闭资源,释放锁等 
        一般来说,代码肯定会执行,特殊情况,例如在执行finally之前jvm退出了
    finalize：是Object类的一个方法,会在垃圾回收对象前调用,来释放资源,每个对象的finalize方法只会被GC调用一次
        GC根据GCroot算法进行来分析对象的可达性来判断对象是否存活,如果不可达被判定为垃圾对象后,会先判断该对象是否覆盖finalize方法,
        如果没有覆盖说明对象不需要经过特殊处理,可以直接回收,否则会将该对象放入一个F-Queue队列中,会被一个低优先级的线程调用,再次进行
        可达性分析,来判断是否复活还是回收.不建议使用,容易引起挂起和死锁.
        
    ```
- string、sringbuffer、stringbuilder的区别
    ```
    String：长度和内容不可变的,内部被final修饰,缓存于字符串常量池中,底层结构是char[]
        String对象是否真的不可变?可通过反射进行改变
            String s = "Hello World"; //创建字符串"Hello World",并赋给引用变量s
            System.out.println("s=" + s);
            Field valueFieldOfString = String.class.getDeclaredField("value"); //获取String类的value字段
            valueFieldOfString.setAccessible(true); //改变value属性的访问权限
            char[] value = (char[]) valueFieldOfString.get(s); //获取s对象上的value属性值
            value[5] = '_'; //改变value所应用的数组中的第5个字符
            System.out.println("s=" + s);
    StringBuffer、StringBuilder：长度和内容可变的,内部被final修饰,继承自AbstractStringBuilder,默认大小是16,可指定,底层结构是char[]
        append操作：会先判断char[]的总长度是否能容纳新添加的字符串,不够的话会进行扩容,默认扩容至是"value.length * 2 + 2",如果该大小
        小于最终需要的长度,则将后者设为需要扩容的长度,调用System.arraycopy方法来实现数组拷贝
    应用场景：
        String：适用于内容不经常发生改变的场景,例如常量声明,少量的字符串拼接操作等
        StringBuffer：适用于频繁进行字符串的运算(拼接,替换,删除等),并且运行多线程环境下,例如XML解析,HTTP参数解析与封装等
            线程安全,效率低,采用synchronized
        StringBuilder：适用于频繁进行字符串的运算(拼接,替换,删除等),并且运行单线程环境下,例如SQL语句拼装,JSON封装等
            线程不安全,效率高
    ```
- HashMap的原理实现
    ```
    底层数据结构为哈希表/散列表,是一个元素为链表的数组,链表中的Node包括hash,key,value,next
    hashmap是非线程安全的,因为put的时候有个扩容操作,会有一个复制数组的操作,有可能会操作旧的数组
    jdk8对hashmap加入了红黑树,在链表的长度>8的时候会进行一个红黑树的转换
    hashtable是线程安全的,get与put操作都是用synchronized实现的,会锁住整个表.
    ConcurrentHashMap是线程安全的
        jdk1.7采用ReentrantLock锁住每个segment;
        jdk1.8采用CAS+Synchronized只针对put上锁,如果key对应的元素不存在,则通过CAS进行操作,否则使用synchronized进行操作;get不上锁,因为Node的成员val是用volatile修饰.
        这也是它比其他并发集合比如hashtable、用Collections.synchronizedMap()包装的hashmap安全效率高的原因之一。
    ```
- synchronized和ReentrantLock区别
    ```
    可重入性：
        都是可重入锁/递归锁,指的是同一线程外层函数获得锁之后，内层递归函数仍然有获取该锁的代码，但不受影响。
    锁的实现：
        Synchronized是依赖于JVM实现的，而ReenTrantLock是JDK实现的，前者的实现是比较难见到的，后者有直接的源码可供阅读。
    性能区别：
        在Synchronized优化以前，synchronized的性能是比ReenTrantLock差很多的，但是自从Synchronized引入了偏向锁，轻量级锁（自旋锁）后，
        两者的性能就差不多了，在两种方法都可用的情况下，官方甚至建议使用synchronized，其实synchronized的优化我感觉就借鉴了ReenTrantLock中的
        CAS技术。都是试图在用户态就把加锁问题解决，避免进入内核态的线程阻塞。
    功能区别：
        很明显Synchronized的使用比较方便简洁，并且由编译器去保证锁的加锁和释放，而ReenTrantLock需要手工声明来加锁和释放锁，为了避免忘记手工释放
        锁造成死锁，所以最好在finally中声明释放锁。
    ReenTrantLock独有的能力：
        1.ReenTrantLock可以通过带布尔值的构造函数指定是公平锁还是非公平锁。而synchronized只能是非公平锁。所谓的公平锁就是先等待的线程先获得锁。
            公平锁是指多个线程在等待同一个锁时，必须按照申请的时间顺序来依次获得锁；而非公平锁则不能保证这一点。非公平锁在锁被释放时，任何一个等待锁的线程都有机会获得锁。
        2.ReenTrantLock提供了一个Condition（条件）类，用来实现分组唤醒需要唤醒的线程们，而不是像synchronized要么随机唤醒一个线程要么唤醒全部线程。
        3.ReenTrantLock提供了一种能够中断等待锁的线程的机制，通过lock.lockInterruptibly()来实现这个机制。
    什么情况下使用ReenTrantLock：如果你需要实现ReenTrantLock的三个独有功能时。  
    ```
    ```
    自旋锁：
        如果持有锁的线程能在很短时间内释放锁资源，那么那些等待竞争锁的线程就不需要做内核态和用户态之间的切换进入阻塞挂起状态，它们只需要等一等（自旋），
        等持有锁的线程释放锁后即可立即获取锁，这样就避免用户线程和内核的切换的消耗。
        但是线程自旋是需要消耗cpu的，说白了就是让cpu在做无用功，线程不能一直占用cpu自旋做无用功，所以需要设定一个自旋等待的最大时间。
    ```
- equals和==的区别
    ```
    ==：是一个比较运算符,基本类型比较的是值,引用类型比较的是地址值(是否为同一个对象的引用).
    equals：是Object类的一个方法,只能比较引用类型,重写前比较的是地址值,重写后一般是比较对象的属性.
    ```

### 并发编程专题

- 线程池相关
    ```
    线程池作用：
        Java中的线程池是运用场景最多的并发框架，几乎所有需要异步或并发执行任务的程序都可以使用线程池。在开发过程中，合理地使用线程池能够带来3个好处。
        第一：降低资源消耗。通过重复利用已创建的线程降低线程创建和销毁造成的消耗。
        第二：提高响应速度。当任务到达时，任务可以不需要等到线程创建就能立即执行。
        第三：提高线程的可管理性。线程是稀缺资源，如果无限制地创建，不仅会消耗系统资源，还会降低系统的稳定性，使用线程池可以进行统一分配、调优和监控。
    Executor框架的基本组成：
        Executor框架的最顶层实现是ThreadPoolExecutor类,Executors工厂类中提供的newFixedThreadPool等方法其实也只是ThreadPoolExecutor的构造函数参数不同而已。
            ThreadPoolExecutor 
                extends AbstractExecutorService 
                    implements ExecutorService
                        extends Executor
        ThreadPoolExecutor参数：
            corePoolSize：核心池的大小
            maximumPoolSize：最大线程数
            keepAliveTime：当线程数大于核心时,多余的空闲线程等待新任务的最长时间
            unit：keepAliveTime的时间单位
            workQueue：用来储存等待执行任务的队列
            threadFactory：线程工厂
            RejectedExecutionHandler：饱和/拒绝策略
    线程池四种创建方式：
        Java通过Executors（jdk1.5并发包）提供四种线程池，分别为：
        newCachedThreadPool 创建一个可缓存线程池，如果线程池长度超过处理需要，可灵活回收空闲线程，若无可回收，则新建线程。
            线程池为无限大，当执行第二个任务时第一个任务已经完成，会复用执行第一个任务的线程，而不用每次新建线程。
        newFixedThreadPool 创建一个定长线程池，可控制线程最大并发数，超出的线程会在队列中等待。
            定长线程池的大小最好根据系统资源进行设置。如Runtime.getRuntime().availableProcessors()
        newScheduledThreadPool 创建一个定长线程池，支持定时及周期性任务执行。
        newSingleThreadExecutor 创建一个单线程化的线程池，它只会用唯一的工作线程来执行任务，保证所有任务按照指定顺序(FIFO, LIFO, 优先级)执行。
    线程池大小选择策略：
        具体根据实际情况具体分析,可通过压测实验
        CPU密集型：cpu核心数 + 1
        IO密集型：cpu核心数 * (1 + 平均等待时间/平均工作时间)
    ```
- 常见并发类
    ```
    CountDownLatch：计数器
        CountDownLatch可以实现类似计数器的功能，计数器的初始值为线程的数量。每当一个线程完成了自己的任务后，计数器的值就会减1。当计数器值到达0时，它表示所有的线程已经完成了任务，然后在闭锁上等待的线程就可以恢复执行任务。
        比如有一个任务A，它要等待其他4个任务执行完毕之后才能执行，此时就可以利用CountDownLatch来实现这种功能了。
        常用方法：
            countDown：计数器-1
            await：減去为0,恢复任务继续执行
    Semaphore：计数信号量
        Semaphore是一种基于计数的信号量。它可以设定一个阈值，基于此，多个线程竞争获取许可信号，完成任务后归还，超过阈值后，线程申请许可信号将会被阻塞。
        Semaphore可以用来构建一些对象池，资源池之类的，比如数据库连接池，我们也可以创建计数为1的Semaphore，将其作为一种类似互斥锁的机制，这也叫二元信号量，表示两种互斥状态。
        常用方法：
            availablePermits：获取当前可用的资源数量
            acquire：申请许可
            release：释放许可
    ```
    ```
    并发队列Queue：
        ConcurrentLinkedQueuｅ：基于链表的高性能非阻塞队列
            通过无锁的方式，实现了高并发状态下的高性能，通常ConcurrentLinkedQueue性能好于BlockingQueue.它是一个基于链接节点的无界线程安全队列。该队列的元素遵循先进先出的原则。
            常用方法：
                add 和offer() 都是加入元素的方法(在ConcurrentLinkedQueue中这俩个方法没有任何区别)
                poll() 和peek() 都是取头元素节点，区别在于前者会删除元素，后者不会。
        BlockingQueue：阻塞队列
            ArrayBlockingQueue：基于数组的并发阻塞队列
                ArrayBlockingQueue是一个有边界的阻塞队列，它的内部实现是一个数组。有边界的意思是它的容量是有限的，我们必须在其初始化的时候指定它的容量大小，容量大小一旦指定就不可改变。
                ArrayBlockingQueue是以先进先出的方式存储数据，最新插入的对象是尾部，最新移出的对象是头部。
            LinkedBlockingQueue：基于链表的FIFO阻塞队列
                LinkedBlockingQueue阻塞队列大小的配置是可选的，如果我们初始化时指定一个大小，它就是有边界的，如果不指定，它就是无边界的。说是无边界，其实是采用了默认大小为Integer.MAX_VALUE的容量。
                它的内部实现是一个链表。和ArrayBlockingQueue一样，LinkedBlockingQueue 也是以先进先出的方式存储数据，最新插入的对象是尾部，最新移出的对象是头部。
            PriorityBlockingQueue：带优先级的无界阻塞队列
                PriorityBlockingQueue是一个没有边界的队列，它的排序规则和 java.util.PriorityQueue一样。
            SynchronousQueue：并发同步阻塞队列
                SynchronousQueue队列内部仅允许容纳一个元素。当一个线程插入一个元素后会被阻塞，除非这个元素被另一个线程消费。
    ```
- 线程间通信
    ```
    多个线程处理同一资源(共享数据),但是任务(处理方式)却不同.
    等待唤醒机制
        Object类提供了3个方法：
            wait(): 让线程处于等待状态，被wait的线程会被存储到线程池中。
            notify(): 唤醒线程池中一个线程(任意)。如果对象调用了notify方法就会通知某个正在等待这个对象的控制权的线程可以继续运行。
            notifyAll(): 唤醒线程池中的所有线程。如果对象调用了notifyAll方法就会通知所有等待这个对象控制权的线程继续运行。
        以上方法为什么定义在Object类中?
            这些方法的调用必需通过锁对象调用,而锁对象有可能是任意对象,所以这些方法必须定义在Object类中.
    ```

### 性能调优专题

- 零拷贝
    ```
    概念：
        零拷贝技术是指计算机执行操作时,CPU不需要先将数据从某处内存复制到另一个特定区域.这种技术通常用于通过网络传输文件节省CPU周期和网络带宽.
    场景：
        kafaka,netty,rocketmq,nginx,apache中都用到了零拷贝。
    好处：
        Linux IO流程：先通过DMA拷贝将磁盘数据读入内核空间缓冲区,然后通过CPU拷贝从内核向用户进程空间缓冲区复制数据.
        减少传输数据在存储器之间不必要的中间拷贝次数,从而有效提高数据传输效率
        减少了用户进程空间和内核空间之间因为上下文切换而带来的开销
    实现：
        1.java NIO - MappedByteBuffer,适合比较大的文件
            FileChannel fileChannel = new RandomAcessFile(new File("data.zip"),"rw").getChannel();
            MappedByteBuffer buffer = fileChannel.map(FileChannel.MapMode.READ_ONLY,0,fileChannel.size());
        2.java NIO - FileChannel.transferTo,直接将当前通道内容传输到另一个通道,没有涉及到Buffer的任务操作,底层通过系统调用sendfile()
            FileChannel fileChannel = new RandomAcessFile(new File("data.zip"),"rw").getChannel();
            SocketChannel socketChannel = SocketChannel.open(new InetSocketAddress("",1234));
            fileChannel.transferTo(0,fileChannel.size(),socketChannel);
    ```

### 数据结构与算法专题

- B树(B-树)与B+树的区别
    ```
    B树与B+树的出现是为了减少磁盘IO的次数,基本思想就是
        降低树的深度,每个节点存储多个元素
        摒弃二叉树结构,采用多叉树
    B树：也叫平衡多路查找树
        每个节点都存储key和data,所有节点组成这颗树,并且叶子节点指针为null.
    B+树：
        只有叶子节点存储data,叶子节点包含了这棵树的所有键值,叶子节点不存储指针.
            这意味着相同大小的磁盘页每个节点可以存储更多元素,使得查询的IO次数更少.
        每个叶子节点增加了顺序访问指针(一个指向相邻叶子节点的指针).
            使得所有节点形成有序链表,便于范围查询,这样一棵树成了数据库系统实现索引的首选数据结构。
    ```

### 设计模式专题

- 如何高效实现单例设计模式
    ```
    核心思想：
        保证系统中一个类仅有一个实例,并且提供一个访问该实例的全局访问方法
    常见应用场景：
        windows任务管理器,数据库连接池,java中的runtime,spring中bean的默认生命周期
    优点：
        避免对象的频繁创建和销毁,提高性能
    实现：
        饿汉式：
            优点是访问性能高,线程安全;缺点是加载时就初始化,可能会造成资源浪费.
        懒汉式：
            优点是访问性能高,延迟初始化,提高了资源利用率;缺点是非线程安全
        懒汉式 + synchronized：
            优点是线程安全,延迟初始化,提高了资源利用率;缺点是getInstance()访问需要同步,并发访问性能低.
        懒汉式 + double check + volatile：
            优点是线程安全,延迟初始化,提高了资源利用率,getInstance()访问性能高.
            public static Singleton getInstance(){
                if(instance == null){
                    synchronized(Singleton.class){
                        if(instance == null){
                            instance = new Singleton();
                        }
                    }
                }
                return instance;
            }
        内部内Holder：
            优点是线程安全,延迟初始化,提高了资源利用率,getInstance()访问性能高.
            public final class Singleton{
                private Singleton(){}
                private static class Holder{
                    private static Singleton INSTANCE = new Singleton();
                }
                public static Singleton getInstance(){
                    return Holder.INSTANCE;
                }
            }
        枚举(Enum)：
            优点是线程安全,getInstance()访问性能高;缺点是不能延迟初始化
            防止通过反射调用私有构造器来创建多个实例;提供了自动序列化机制,防止反序列化的时候创建新的对象.
            public enum Singleon{
                INSTANCE;
                void otherMethod(){...}
            }
    ```

### 互联网工具专题

### 源码框架专题

### 分布式框架专题

- 分布式事务解决方案
    ```
    本地事务：
    分布式事务：
        单JVM跨库事务：
            XA/JTA两阶段提交方案：
                开源框架实现有atomikos,外加补偿机制.在CAP理论当中强调的是一致性,由于可用性较低,实际应用的并不多.
        多JVM微服务：
            CAP理论：
                开发大规模分布式系统会遇到一致性,可用性,分区容错性3个特性,而一个分布式系统最多只能满足其中的2项.而分区容错性是分布式系统
                必然要面对和解决的问题,因此我们要根据业务特点在一致性和可用性之间寻求平衡.
                分区容错性：在任意分区网络故障的情况下系统仍能继续运行
            BASE理论：
                基本可用：指分布式系统在出现不可预知故障的时候,允许损失部分可用性.
                软状态：允许系统中的数据存在中间状态,即允许数据同步的过程中存在延时.
                最终一致：本质是需要系统保证最终数据能够达到一致,而不需要实时保证系统数据的强一致性.
            柔性事务：
                最大努力通知方案： 
                TCC两阶段补偿性方案：
                    try：完成所有业务检查(一致性),预留业务资源(准隔离性)
                    confrim：确认执行业务操作,不做任务业务检查,只使用try阶段预留的业务资源
                    cancel：取消try阶段预留的业务资源
                    开源框架实现：atomikos商业版,tcc-transaction,spring-cloud-rest-tcc,支付宝tcc(XTS)
                        分布式事务框架主要是帮我们解决了第二阶段的自动化;充当了一个协调器,根据预留操作的返回结果来自动化调用提交/取消操作.
                可靠消息最终一致性方案：
                    基于RocketMQ：存在中间状态
                    基于普通消息队列：不存在中间状态,自己单独写个服务来实现中间状态
        TCC与XA对比：
            XA是资源(数据库)层面的分布式事务,强一致性,在两阶段提交的整个过程中,一直会持有资源的锁.
            TCC是业务层面的分布式事务,最终一致性,不会一直持有资源的锁.针对整个系统提升了性能.
    ```

## 参考链接

## 结束语

- 未完待续...