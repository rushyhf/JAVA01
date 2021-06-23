## 第五章：

### 作业：

1.多线程计算加法：

代码：

```java
/**
        先前的兄弟写得太好了。。
        借鉴了一下下。。
        **/
public class Calc {
    volatile static int num = 1;
    volatile static int total = 0;

    public static void main(String[] args) {
        int remain = 10000 / 6;
        int mod = 10000 % 6;

        for (int i = 0; i < 6; i++) {
            if (mod > 0) {
                new Thread(new CountThread(remain + 1), String.valueOf(i)).start();
                mod--;
            } else {
                new Thread(new CountThread(remain), String.valueOf(i)).start();
            }
        }
    }
}

class Add {
    public static synchronized void add() {
        Calc.total += Calc.num;
        Calc.num += 1;
        System.out.println(Calc.total);
    }
}

class CountThread implements Runnable {
    Add add = new Add();
    volatile int cnt;

    public CountThread(int cnt) {
        this.cnt = cnt;
    }

    @Override
    public void run() {
        while (cnt != 0) {
            if (cnt > 0) {
                add.add();
                cnt--;
            }
        }
    }
}public class Calc {
    volatile static int num = 1;
    volatile static int total = 0;

    public static void main(String[] args) {
        int remain = 100 / 6;
        int mod = 100 % 6;

        for (int i = 0; i < 6; i++) {
            if (mod > 0) {
                new Thread(new CountThread(remain + 1), String.valueOf(i)).start();
                mod--;
            } else {
                new Thread(new CountThread(remain), String.valueOf(i)).start();
            }
        }
    }
}

class Add {
    public static synchronized void add() {
        Calc.total += Calc.num;
        Calc.num += 1;
        System.out.println(Calc.total);
    }
}

class CountThread implements Runnable {
    Add add = new Add();
    volatile int cnt;

    public CountThread(int cnt) {
        this.cnt = cnt;
    }

    @Override
    public void run() {
        while (cnt != 0) {
            if (cnt > 0) {
                add.add();
                cnt--;
            }
        }
    }
}
```

运行截图：

![](data:image/png;base64,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)

### 笔记：

#### 多进程和多线程简介：

+ 多进程概念：
  + 当前的操作系统都是多任务OS
  + 每个独立执行的任务就是一个进程
  + OS将时间划分为多个时间片（时间很短）
  + 每个时间片内将CPU分配给某一个任务，时间片结束，CPU将自动回收，再分配给另外任务。从外部看，所有任务是同时在执行。但是在CPU上，任务是按照串行依次运行（单核CPU）。如果是多核，多个进程任务可以并行。但是单个核上，多进程只能串行执行。

+ 多进程的优点
  + 可以同时运行多个任务
  + 程序因IO堵塞时，可以释放CPU，让CPU为其他程序服务
  + 当系统有多个CPU时，可以为多个程序同时服务
    + 我们的CPU不再提高频率，而是提高核数
    + 2005年Herb Sutter的文章The free lunch is over，指明多核和并行程序才是
      提高程序性能的唯一办法
+ 多进程的缺点
  + 太笨重，不好管理
  + 太笨重，不好切换

+ 多线程概念
  + 一个程序可以包括多个子任务，可串/并行
  + 每个子任务可以称为一个线程
  + 如果一个子任务阻塞，程序可以将CPU调度另外一个子任务进行工作。这样CPU还是保留在本程序中，而不是被调度到别的程序（进程）去。这样，提高本程序所获得CPU时间和利用率。

+ 多进程和多线程对比
  + 线程共享数据
  + 线程通讯更高效
  + 线程更轻量级，更容易切换
  + 多个线程更容易管理

+ 总结
  + 了解多进程和多线程基础概念
  + 了解多进程和多线程的特点和适用场合



#### Java多线程实现：

+ Java多线程创建

  + java.lang.Thread
    + 线程继承Thread类，实现run方法
  + java.lang. Runnable接口
    + 线程实现Runnable接口，实现run方法

+ Java多线程启动

  + 启动
    + start方法，会自动以新进程调用run方法
    + 直接调用run方法，将变成串行执行
    + 同一个线程，多次start会报错，只执行第一次start方法
    + 多个线程启动，其启动的先后顺序是随机的
    + 线程无需关闭，只要其run方法执行结束后，自动关闭
    + main函数（线程）可能早于新线程结束，整个程序并不终止
    + 整个程序终止是等所有的线程都终止（包括main函数线程）

+ Java多线程实现对比

  + Thread占据了父类的名额，不如Runnable方便

  - Thread类实现Runnable

  + Runnable启动时需要Thread类的支持
  + Runnable更容易实现多线程中资源共享
  + 结论：建议实现Runnable接口来完成多线程

+ 总结

  + 了解Java多线程两种实现方式
  + 了解Java多线程运行基本规则



#### Java多线程信息共享：

+ 多线程信息共享
  + 线程类
  + 通过继承Thread或实现Runnable
  + 通过start方法， 调用run方法， run方法工作
  + 线程run结束后，线程退出
+ 粗粒度：子线程与子线程之间、和main线程之间缺乏交流
+ 细粒度：线程之间有信息交流通讯
  + 通过共享变量达到信息共享
  + JDK原生库暂不支持发送消息（类似MPI并行库直接发送消息）
+ 通过共享变量在多个线程中共享消息
  + static变量
  + 同一个Runnable类的成员变量
+ 多线程信息共享问题
  + 工作缓存副本
  + 关键步骤缺乏加锁限制
+ i+＋，并非原子性操作
  + 读取主存i（正本）到工作缓存（副本）中
  + 每个CPU执行（副本）i+1操作
  + CPU将结果写入到缓存（副本）中
  + 数据从工作缓存（副本）刷到主存（正本）中
+ 变量副本问题的解决方法
  + 采用volatile关键字修饰变量
  + 保证不同线程对共享变量操作时的可见性
+ 关键步骤加锁限制
  + 互斥：某一个线程运行一个代码段（关键区），其他线程不能同时运行这个代码段
  + 同步：多个线程的运行，必须按照某一种规定的先后顺序来运行
  + 互斥是同步的一种特例
+ 互斥的关键字是synchronized
  + synchronized代码块/函数，只能一个线程进入
  + synchronized加大性能负担，但是使用简便

+ 总结
  + 了解多线程之间信息共享机制
  + 掌握volatile和synchronized的用法



#### Java多线程管理：

+ 线程类
  + 通过继承Thread或实现Runnable
  + 通过start方法，调用run方法，run方法工作
  + 线程run结束后，线程退出
+ 粗粒度：子线程与子线程之间、和main线程之间缺乏同步
+ 细粒度：线程之间有同步协作
  + 等待
  + 通知/唤醒
  + 终止

+ 线程状态
  + NEW刚创建（new）
  + RUNNABLE就绪态（start）
  + RUNNING运行中（run）
  + BLOCK阻塞（sleep）
  + TERMINATED 结束

+ Thread的部分API已经废弃
  + 暂停和恢复suspend /resume
  + 消亡stop/destroy
+ 线程阻塞/和唤醒
  + sleep，时间一到，自己会醒来
  + wait/notify / notifyAll，等待，需要别人来唤醒
  + join，等待另外一个线程结束
  + interrupt，向另外一个线程发送中断信号，该线程收到信号，会触发InterruptedException（可解除阻塞），并进行下一步处理

+ 线程被动地暂停和终止
  + 依靠别的线程来拯救自己
  + 没有及时释放资源
+ 线程主动暂停和终止
  + 定期监测共享变量
  + 如果需要暂停或者终止，先释放资源，再主动动作
  + 暂停：Thread.sleep（），休眠
  + 终止： run方法结束，线程终止

+ 多线程死锁
  + 每个线程互相持有别人需要的锁（哲学家吃面问题）
  + 预防死锁，对资源进行等级排序
+ 守护（后台）线程
  + 普通线程的结束，是run方法运行结束
  + 守护线程的结束，是run方法运行结束，或main函数结束
  + 守护线程永远不要访问资源，如文件或数据库等
+ 线程查看工具 jvisualvm

+ 总结
  + 了解线程的多个状态
  + 了解线程协作机制
  + 线程协作尽量简单化，采用粗粒度协作
  + 了解死锁和后台线程概念
  + 使用jvisualvm查看线程执行情况



## 第五章（续）：

### 作业：

1.多线程计数：

代码：

```java
import java.util.concurrent.*;

public class AdvancedCalc {
    public static void main(String[] args) throws Exception {
        int[] numbers = new int[10000];
        int search = 50;

        for (int i = 0; i < numbers.length; i++) {
            numbers[i] = (int) (Math.random() * 101);
        }

        System.out.println("串行搜索得到50的个数是" + useSerial(numbers, search) + "个。");
        System.out.println("Executors搜索得到50的个数是" + useExecutor(numbers, search) + "个。");
        System.out.println("Fork-Join搜索得到50的个数是" + useForkJoin(numbers, search) + "个。");
    }

    public static int useSerial(int[] numbers, int search) {
        int cnt = 0;
        for (int number : numbers) {
            if (number == search) {
                cnt++;
            }
        }
        return cnt;
    }

    public static int useExecutor(int[] numbers, int search) throws Exception {
        ExecutorService executor = Executors.newCachedThreadPool();

        int increment = 1000;
        int numOfThread = numbers.length / increment;
        CountDownLatch latch = new CountDownLatch(numOfThread);

        int startIndex = 0, endIndex = increment - 1;

        for (int i = 0; i < numOfThread; i++) {
            if (i == numOfThread - 1) {
                endIndex = numbers.length - 1;
            }
            executor.execute(new Executor(numbers, startIndex, endIndex, search, latch));
            startIndex = endIndex + 1;
            endIndex = endIndex + increment;
        }

        latch.await();
        executor.shutdown();
        return Executor.getSumCnt();
    }

    public static int useForkJoin(int[] numbers, int search) throws Exception {
        ForkJoinPool forkJoinPool = new ForkJoinPool();
        ForkJoinTask<Integer> forkJoinTask = new ForkJoin(numbers, 0, numbers.length - 1, search);

        forkJoinTask = forkJoinPool.submit(forkJoinTask);
        Integer cnt = forkJoinTask.get();
        forkJoinPool.shutdown();
        return cnt;
    }
}

class Executor implements Runnable {
    private static int sumCnt = 0;
    private int[] numbers;
    private int startIndex;
    private int endIndex;
    private int search;
    private CountDownLatch latch;

    public Executor(int[] numbers, int startIndex, int endIndex, int search, CountDownLatch latch) {
        this.numbers = numbers;
        this.startIndex = startIndex;
        this.endIndex = endIndex;
        this.search = search;
        this.latch = latch;
    }

    public static int getSumCnt() {
        return sumCnt;
    }

    @Override
    public void run() {
        int cnt = 0;
        for (int i = startIndex; i <= endIndex; i++) {
            if (numbers[i] == search) {
                cnt++;
            }
        }

        synchronized (Executor.class) {
            sumCnt += cnt;
        }
        latch.countDown();
    }
}

class ForkJoin extends RecursiveTask<Integer> {
    int[] numbers;
    int startIndex;
    int endIndex;
    int search;

    public ForkJoin(int[] numbers, int startIndex, int endIndex, int targetNumber) {
        this.numbers = numbers;
        this.startIndex = startIndex;
        this.endIndex = endIndex;
        this.search = targetNumber;
    }

    @Override
    protected Integer compute() {
        int cnt = 0;
        if (endIndex - startIndex < 1000) {
            for (int i = startIndex; i <= endIndex; i++) {
                if (numbers[i] == search) {
                    cnt++;
                }
            }
        } else {
            int midIndex = (startIndex + endIndex) / 2;
            ForkJoin left = new ForkJoin(numbers, startIndex, midIndex, search);
            ForkJoin right = new ForkJoin(numbers, midIndex + 1, endIndex, search);

            invokeAll(left, right);
            cnt = right.join() + left.join();
        }
        return cnt;
    }
}
```

运行截图：

![](data:image/png;base64,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)

### 笔记：

#### Java并发框架Executor：

+ 并行计算
  + 业务：任务多，数据量大·串行vs并行
    + 串行编程简单，并行编程困难
    + 单个计算核频率下降，计算核数增多，整体性能变高
  + 并行困难（任务分配和执行过程高度耦合）
    + 如何控制粒度，切割任务
    + 如何分配任务给线程，监督线程执行过程
  + 并行模式
    + 主从模式（Master-Slave）
    + Worker模式（Worker-Worker）
  + Java并发编程
    + Thread/Runnable / Thread组管理
    + Executor（本节重点）
    + Fork-Join框架
  + 线程组管理
    + 线程组ThreadGroup
    + 线程的集合
    + 树形结构，大线程组可以包括小线程组
    + 可以通过enumerate方法遍历组内的线程，执行操作一能够有效管理多个线程，但是管理效率低
    + 任务分配和执行过程高度耦合
    + 重复创建线程、关闭线程操作，无法重用线程
+ Executor
  + 从JDK5开始提供Executor FrameWork （java.util.concurrent.*）
    + 分离任务的创建和执行者的创建
    + 线程重复利用（new线程代价很大）
  + 理解共享线程池的概念
    + 预设好的多个Thread，可弹性增加
    + 多次执行很多很小的任务
    + 任务创建和执行过程解耦
    + 程序员无需关心线程池执行任务过程
  + 主要类：ExecutorService， ThreadPoolExecutor，Future
    + Executors.newCachedThreadPool/newFixcdThrcadPool创建线程池
    + ExecutorScrvice 线程池服务
    + Callable具体的逻辑对象（线程类）
    + Future 返回结果

+ 总结
  + 掌握共享线程池原理
  + 熟悉Executor框架，提高多线程执行效率



#### Java并发框架Fork-Join：

+ Fork-Join
  + Java 7提供另一种并行框架︰分解、治理、合并（分治编程）
  + 适合用于整体任务量不好确定的场合（最小任务可确定）
  + 关键类
    + ForkJoinPool任务池
    + RecursiveAction
    + RecursiveTask

+ 总结
  + 了解Fork-Join和Exccutor的区别
  + 控制任务分解粒度
  + 熟悉Fork-join框架，提高多线程执行效率



#### Java并发数据结构：

+ 并发数据结构
  + 常用的数据结构是线程不安全的
    + ArrayList， HashMap， HashSet非同步的
    + 多个线程同时读写，可能会抛出异常或数据错误
  + 传统Vector，Hashtable等同步集合性能过差
  + 并发数据结构：数据添加和删除
    + 阻塞式集合：当集合为空或者满时，等待
    + 非阻塞式集合：当集合为空或者满时，不等待，返回null或异常

- List
  - Vector同步安全，写多读少
  - ArrayList不安全
  - Collections.synchronizedList（List list）基于synchronized，效率差
  - CopyOnWritcArrayList读多写少，基于复制机制，非阻塞
- Set
  - HashSet不安全
  - Collections.synchronizedSet（Set set）基于synchronized，效率差
  - CopyOnWriteArraySet（基于CopyOnWriteArrayList实现）读多写少，非阻塞

+ Map
  + Hashtable同步安全，写多读少
  + HashMap不安全
  + Collections.synchronizedMap（Map map）基于synchronized，效率差
  + ConcurrentHashMap读多写少，非阻塞
+ Queue & Deque（队列，JDK1.5提出）
  + ConcurrentLinkedQueue非阻塞
  + ArrayBlockingQueue/LinkedBlockingQueue阻塞

+ 总结
  + 了解数据结构并发读写的问题
  + 根据业务特点，使用正确的并发数据结构



#### Java并发协作控制：

+ 线程协作
  + Thread/Executor/Fork-Join
    + 线程启动，运行，结束
    + 线程之间缺少协作
  + synchronized同步
    + 限定只有一个线程才能进入关键区
    + 简单粗暴，性能损失有点大

+ Lock
  + Lock也可以实现同步的效果
    + 实现更复杂的临界区结构
    + tryLock方法可以预判锁是否空闲
    + 允许分离读写的操作，多个读，一个写
    + 性能更好
  + ReentrantLock类，可重入的互斥锁
  + ReentrantReadWriteLock类，可重入的读写锁
  + lock和unlock函数

+ Semaphore
  + 信号量，由1965年Dijkstra提出的
  + 信号量：本质上是一个计数器
  + 计数器大于0，可以使用，等于0不能使用·可以设置多个并发量，例如限制10个访问 
  + Semaphore
    + acquirc获取
    + release释放
  + 比Lock更进一步，可以控制多个同时访问关键区

+ Latch
  + 等待锁，是一个同步辅助类
  + 用来同步执行任务的一个或者多个线程
  + 不是用来保护临界区或者共享资源
  + CountDownLatch
    - countDown（）计数减1
    - await（）等待latch变成0

+ Barrier
  + 集合点，也是一个同步辅助类
  + 允许多个线程在某一个点上进行同步
  + CyclicBarrier
    + 构造函数是需要同步的线程数量
    + await等待其他线程，到达数量后，就放行

+ Phaser
  + 允许执行并发多阶段任务，同步辅助类
  + 在每一个阶段结束的位置对线程进行同步，当所有的线程都到达这步，再进行下一步。
  + Phaser
    + arrive（）
    + arriveAndAwaitAdvance（）

+ Exchanger
  + 允许在并发线程中互相交换消息
  + 允许在2个线程中定义同步点，当两个线程都到达同步点，它们交换数据结构
  + Exchanger
    + exchange（），线程双方互相交互数据
    + 交换数据是双向的

+ 总结
  + java.util.concurrcnt包提供了很多并发编程的控制类
  + 根据业务特点，使用正确的线程并发控制



#### Java定时任务执行：

+ 定时任务
  + Thread/Executor/Fork-Join多线程
    + 立刻执行
    + 框架调度
  + 定时执行
    + 固定某一个时间点运行
    + 以某一个周期
  + 简单定时器机制
    + 设置计划任务，也就是在指定的时间开始执行某一个任务。
    + TimerTask封装任务
    + Timer类定时器
  + Executor＋定时器机制
  + ScheduledExecutorService
    + 定时任务
    + 周期任务

+ Quartz
  - Quartz是一个较为完善的任务调度框架
  - 解决程序中Timer零散管理的问题
  - 功能更加强大
    - Timer执行周期任务，如果中间某一次有异常，整个任务终止执行
    - Quartz执行周期任务，如果中间某一次有异常，不影响下次任务执行
    - ......

+ 总结
  + 了解定时任务的作用
  + 根据业务特点，使用合适的定时任务器