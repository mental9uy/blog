# juc

- [juc](#juc)
  - [一、多线程入门](#一多线程入门)
    - [1.1 线程安全问题](#11-线程安全问题)
  - [二、内存模型（JMM）](#二内存模型jmm)
    - [2.1 内存架构](#21-内存架构)
    - [2.2 缓存一致性问题](#22-缓存一致性问题)
    - [2.3 处理器优化和指令重排序](#23-处理器优化和指令重排序)
    - [2.4、并发编程的问题](#24并发编程的问题)
  - [三、CAS 原理](#三cas-原理)
    - [3.1 CAS 概述](#31-cas-概述)
    - [3.2 CAS 原理](#32-cas-原理)
    - [3.3 CAS 在 Java 中的应用](#33-cas-在-java-中的应用)
    - [3.4 CAS 存在的问题](#34-cas-存在的问题)
  - [四、Java 中的锁](#四java-中的锁)
    - [4.1 乐观锁&悲观锁](#41-乐观锁悲观锁)
    - [4.2 独占锁&共享锁](#42-独占锁共享锁)
    - [4.3 互斥锁&读写锁](#43-互斥锁读写锁)
    - [4.4 公平锁&非公平锁](#44-公平锁非公平锁)
    - [4.5 可重入锁（递归锁）](#45-可重入锁递归锁)
    - [4.6 自旋锁](#46-自旋锁)
    - [4.7 分段锁](#47-分段锁)
    - [4.8 锁升级](#48-锁升级)
    - [4.9 锁优化技术（粗化、消除）](#49-锁优化技术粗化消除)

---

```md
author: WuDG
datetime： 16:09:07 星期一 2021年8月2日
position： 杭州
```

## 一、多线程入门

### 1.1 线程安全问题
> 单线程环境正常运行的代码，在多线程环境中可能出现意料之外的结果

**1.1 原子性**
问题场景：银行转账，账户 A 向账户 B 转 1000元，，其中包含两个操作：账户 A 减去 1000元，B 账户增加 1000元。前面两个操作都成功才是转账成功
由于这两个操作不具备原子性，因此可能出现账户 A 扣款成功但账户 B 入款失败的情况

> 原子性：多个操作，要么全部执行要不都不执行
原子操作：不会被线程调度机制打断的操作，没有上下文切换

```java
i = 0;  // 只有这个操作是原子操作
i++;
i = j;
i = i + 1;
````
`在 Java 中可以通过使用 synchoniezed 或者 lock 来保证原子性`

**1.2 可见性**
> 指当多个线程访问同一个变量时，一个线程修改了这个变量的值，其他线程能够立即得到修改后的值
工作内存和主内存见数据交互

Java 提供了 volatile 关键词来解决多线程可见性问题
也可通过 synchronized 和 lock 来保证可见性，加锁可以保证同一时刻只有一个线程在执行同步代码块，释放锁之前会将变量刷回至主存

**1.3 活跃性问题**
> 活跃性是指某件正确的事情最终会发生，当某个操作无法继续下去的时候，就会发生活跃性问题
活跃性问题一般包括：死锁、活锁、饥饿问题
加锁使用不当也容易引入其他问题，如“死锁”
* 死锁：资源等待且成环，持有资源
* 活锁：死锁的对立面，都释放资源
* 饥饿：一个线程无其他异常却迟迟不能继续允许（哲学家用餐问题）
  * 高优先级的线程一直在运行消耗 CPU，所有的低优先级的线程一直在等待
  * 一些线程被永久堵塞在一个等待进入同步块的状态，而其他线程总是能在它之前持续的对该同步块进行访问


**1.4 性能问题**
多线程额外耗时：创建线程、线程上下文切换
减少线程上下文切换方法：无锁并发编程、CAS 算法、使用协程

## 二、内存模型（JMM）
> 由于主存与 CPU 处理器的运算能力之间差距较大，所以传统计算机内存架构中会引入高速缓存来作为主存和CPU之间的缓冲，CPU将常用的数据存放在高速缓存中，运算结束后 CPU 再将运算结果同步到主存中

### 2.1 内存架构
* CPU
* 主存
* CPU 缓存
* CPU 寄存器

### 2.2 缓存一致性问题
> 使用高速缓存解决了 CPU 和主存速率不匹配问题后又引入了一个新的问题：缓存一致性问题

每个 CPU 内核都有自己的高速缓存，它们共享主内存。当多个 CPU 的运算任务都涉及同一块主内存区域时，CPU 会将数据读取到缓存中进行运算，可能导致各自的缓存数据不一致

### 2.3 处理器优化和指令重排序
> 为了使处理器内部的运算单元最大化被充分利用，处理器会对输入代码进行重排序执行
提升 CPU 执行效率：处理器优化
除了处理器会对代码进行优化处理，还有部分编译器也会做类似优化，如 Java 的即时编译器（JIT）会做指令重排序

重排序类型：
* 编译器优化的重排序：在不改变单线程语义前提下，可以重新安排语句的执行顺序
* 指令级并行的重排序：现代处理器采用了指令级并行技术来将多条指令重叠执行。如果不存在数据依赖性，处理器可以改变语句对应机器指令的执行顺序
* 内存系统的重排序：由于处理器使用缓存和读写缓冲区，使得加载和存储看起来乱序执行

定义一套内存模型，规范内存的读写操作。内存模型解决并发问题两种方式：限制处理器优化和使用内存屏障
* 所有变量都存储在主内存中
* 每个线程都有一个私有的本地内存，本地内存中存储了该现场以读/写共享变量的拷贝副本
* 线程对变量的所有操作都必须在本地内存进行，不能直接读写主内存
* 不同的线程之间无法直接访问对方本地内存中的变量

为了更好的控制主内存和本地内存的交互，Java 内存模型定义了八种操作：
1. lock：锁定，作用于主内存的变量，把一个变量标识为一条线程独占状态
2. unlock：读取，作用于主内存变量，把一个处于锁定状态的变量释放出来，释放后的变量才可以被其线程锁定
3. read：读取。作用于主内存变量，把一个变量值从主内存传输到线程的工作内存中，以便随后的load动作使用
4. load：载入。作用于工作内存的变量，它把read操作从主内存中得到的变量值放入工作内存的变量副本中。
5. use：使用。作用于工作内存的变量，把工作内存中的一个变量值传递给执行引擎，每当虚拟机遇到一个需要使用变量的值的字节码指令时将会执行这个操作。
6. assign：赋值。作用于工作内存的变量，它把一个从执行引擎接收到的值赋值给工作内存的变量，每当虚拟机遇到一个给变量赋值的字节码指令时执行这个操作。
7. store：存储。作用于工作内存的变量，把工作内存中的一个变量的值传送到主内存中，以便随后的write的操作。
8. write：写入。作用于主内存的变量，它把store操作从工作内存中一个变量的值传送到主内存的变量中。


### 2.4、并发编程的问题
* 缓存一致性问题 -- 可见性问题
* 处理器优化 -- 原子性问题
* 指令重排序 -- 有序性问题  

## 三、CAS 原理
i++ 操作是非线程安全的，因为 i++ 不是原子操作
保证原子性：加锁（synchronized 和 CAS）

* synchronized：悲观锁，线程执行需要先获取锁，结束后再释放锁
* CAS：乐观锁，失败重试

### 3.1 CAS 概述
> Compare-And-Swap：比较并交换，是一条 CPU 并发原语，用于判断内存中某个值是否为预期值，如果是则改为新的值，这个过程是原子的

CAS 机制中三个基本操作数：内存地址 V，旧的预期值 A，计算后要修改后的值 B

总结：更新一个变量的时候，只有当变量的预期值 A 和内存地址 V 中的实际值相同时，才会将内存地址 V 对应的值修改为 B，这整个操作就是CAS

### 3.2 CAS 原理
CAS 主要包括两个操作：Compare 和 Swap
CAS 是一种系统原语，原语由若干指令组成，用于完成某个功能，且原语的执行必须是连续的，由系统硬件保证

### 3.3 CAS 在 Java 中的应用
Java 中通常不会直接使用 CAS，都是通过 JDK 封装好的兵法工具类间接使用，在 `java.util.concurrent` 包中

CAS 主要在 JUC 下面的 atomic 包下
如 AtomicInteger 类就可以解决 i++ 非原子性问题，主要通过 Unsafe 下面的方法 和 volatile 关键词实现原子操作

### 3.4 CAS 存在的问题
* **ABA问题**：A -> B -> A，增加版本号，每次更新变量就把版本号 +1
* **自旋开销**：限制自旋次数，避免过度消耗 CPU
* **只能保证单个变量的原子性**：如果有多个，则只能通过 synchronized 或 lock

## 四、Java 中的锁

### 4.1 乐观锁&悲观锁
* 悲观锁：synchronized 和 ReentrantLock，适合写多读少场景
* 乐观锁：使用 `CAS 算法`和`版本号机制`实现，适合读多写少场景

### 4.2 独占锁&共享锁
* 独占锁：只能被一个线程持有（Lock）
* 共享锁：可被多个线程持有，获取共享锁的线程只能读数据，不能修改数据（ReentrantReadWriteLock）

### 4.3 互斥锁&读写锁
* 互斥锁：类似独占锁和悲观锁
* 读写锁：共享锁的一种具体实现，一个只读的锁和一个写锁，只能一个写线程，可以多个读线程（ReadWriteLock）

### 4.4 公平锁&非公平锁
* 公平锁：指多个线程按照申请锁的顺序来获取锁，类似队列（new ReentrantLock(true);）
* 非公平锁：并不是按照申请锁的顺序，按照现场优先级（new ReentrantLock();）

### 4.5 可重入锁（递归锁）
* 指同一个线程在外层方法获取了锁，在进入内层方法会自动获取锁（ReentrantLock，synchronized）

### 4.6 自旋锁
* 指线程在没有获得锁时不是被直接挂起，而是执行一个忙循环，即自旋（CAS）

### 4.7 分段锁
* 将锁的粒度进一步细化，当操作不需要更新整个数组的时候，就仅仅针对数组中的一项进行枷锁操作（CurrentHashMap）

### 4.8 锁升级
> 减少获得锁和释放锁带来的消耗
无锁 -> 偏向锁 -> 轻量级锁 -> 重量级锁 

* 无锁：即乐观锁
* 偏向锁：偏向于第一个访问锁的线程
* 轻量级锁：线程竞争激烈时，偏向锁升级为轻量级锁，竞争程度很低，通过自旋方式等待上一个线程释放锁
* 重量级锁：线程竞争加剧，自旋超过一定次数，或者有两个线程，一个线程持有锁，一个在自旋，又来了第三个线程访问时，轻量级锁就会膨胀为重量级锁

### 4.9 锁优化技术（粗化、消除）
* 粗化：减少同步块的数量，如对 for 循环中的 synchronized 块进行粗化，把 for 循环放在 synchronized 代码块中
* 细化：线程安全的方法中使用 StringBuffer#append 方法，为了提升销量，虚拟机会消除方法的同步锁

