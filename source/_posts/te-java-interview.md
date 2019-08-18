---
title: Java常见面试题
date: 2019-08-18 16:46:58
categories:
- 经典示例
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

- Exception、Error的区别
    ```
    首先,Exception和Error都继承自Throwable类
    Error类一般是指与虚拟机相关的错误，对于这类错误导致的应用程序中断无法捕获处理，如OutofMemoryError、StackOverflowError等。
    Exception类表示程序可以处理的异常，对于这类异常，应该尽可能处理异常，使程序恢复，分为运行时异常和编译时异常。
        运行时异常：RuntimeException及其子类,可以不处理(可以修改代码更严谨),也可以处理
            NullPointerException、ClassCastException、ClassNotFoundException、IllegalArgumentException
        编译时异常：非RuntimeException,必须要处理,否则编译不通过
            IOException、SQLException
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
        如果没有覆盖说明对象不需要经过特殊处理,可以直接回收,否则会将该对象放入一个F-Queue队列中,会被一个低优先级的对列调用,再次进行
        可达性分析,来判断是否复活还是回收.不建议使用,容易引起挂起和死锁.
        
    ```
 - 异常处理原则
    ```
    尽量捕获特定异常,来快速定位问题,而不是捕获通用异常
    不要生吞异常,而不是捕获后什么都不做
    只捕获有必要的代码段,捕获会耗资源
    不要使用异常处理块控制代码流程
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
    hashtable是线程安全的,get与put操作都是用synchronized实现的
    ConcurrentHashMap是线程安全的;jdk1.7采用ReentrantLock;jdk1.8采用CAS+Synchronized只针对put上锁,get不上锁,因为Node的成员val是用volatile修饰
        这也是它比其他并发集合比如hashtable、用Collections.synchronizedMap()包装的hashmap安全效率高的原因之一。
    ```
- synchronized和ReentrantLock区别
    ```
    可重入性：
        都是可重入锁/递归锁,指的是同一线程外层函数获得锁之后，内层递归函数仍然有获取该锁的代码，但不受影响。
    锁的实现：
        Synchronized是依赖于JVM实现的，而ReenTrantLock是JDK实现的，前者的实现是比较难见到的，后者有直接的源码可供阅读。
    性能的区别：
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

### 性能调优专题

### 数据结构与算法专题

### 设计模式专题

### 互联网工具专题

### 源码框架专题

### 分布式框架专题



## 参考链接

## 结束语

- 未完待续...