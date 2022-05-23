# jvm

- [jvm](#jvm)
  - [一、Java 内存区域（运行时数据区）](#一java-内存区域运行时数据区)
  - [二、HotSpot 对象](#二hotspot-对象)
    - [2.1 Java创建对象的过程](#21-java创建对象的过程)
    - [2.2 对象的内存布局](#22-对象的内存布局)
    - [2.3 对象的访问定位](#23-对象的访问定位)
  - [三、String 类和常量池](#三string-类和常量池)
    - [3.1 String 对象两种创建方式](#31-string-对象两种创建方式)
    - [3.2 基本类型、包装类型、常量池](#32-基本类型包装类型常量池)
  - [四、垃圾回收](#四垃圾回收)
    - [4.1 堆空间基本结构](#41-堆空间基本结构)
    - [4.2 堆内存分配策略](#42-堆内存分配策略)
    - [4.3 对象死亡](#43-对象死亡)
  - [五、引用](#五引用)
  - [六、垃圾收集算法](#六垃圾收集算法)
    - [6.1 标记-清除算法](#61-标记-清除算法)
    - [6.2 标记-复制算法](#62-标记-复制算法)
    - [6.3 标记-整理算法](#63-标记-整理算法)
    - [6.4 分代收集算法](#64-分代收集算法)
  - [七、垃圾收集器](#七垃圾收集器)
    - [7.1 Serial 收集器](#71-serial-收集器)
    - [7.2 ParNew 收集器](#72-parnew-收集器)
    - [7.3 Parallel Scavenge 收集器（JDK8 默认收集器：Parallel Scavenge + Parallel Old）](#73-parallel-scavenge-收集器jdk8-默认收集器parallel-scavenge--parallel-old)
    - [7.4 Serial Old 收集器](#74-serial-old-收集器)
    - [7.5 Parallel Old 收集器](#75-parallel-old-收集器)
    - [7.6 CMS 收集器（Concurrent Mark Sweep）](#76-cms-收集器concurrent-mark-sweep)
    - [7.7 G1 收集器](#77-g1-收集器)
    - [7.8 ZGC 收集器](#78-zgc-收集器)

---

```md
author: WuDG
datetime： 17:28:31 星期四 2021年8月5日
position： 杭州
```

* [JVM 规范](https://docs.oracle.com/javase/specs/jvms/se8/html/index.html)
* 《深入理解Java虚拟机（第三版）》

## 一、Java 内存区域（运行时数据区）
**JVM1.6&1.7运行时数据区域**

**Java运行时数据区域JDK1.8**


**线程私有**
* PC：当前线程所执行的字节码的行号指示器，字节码解释器通过改变 PC 来读取指令。分支、循环、跳转、异常处理、线程恢复都依赖 PC。唯一无 OOM Error
* 虚拟机栈：描述 Java 方法执行，由栈帧组成（局部变量表、操作数栈、动态链接、方法出口信息等）。存在 StackOverFlowError 和 OOM Error
  * 局部变量表L存放编译器可知得数据类型（boolean、byte、char、short、int、float、long、double）、对象引用
* 本地方法栈：运行 native 方法

**线程共享**
* 堆：几乎所有的对象实例和数组都在堆分配
* 方法区（非堆）：存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码数据。
* 直接内存（非运行时数据区部分）

**JVM堆内存结构-JDK7**
etc

**JVM堆内存结构-JDK8**
etc

**运行时常量池**
> 编译器生成的各种字面量和符号引号

**直接内存**
> 不属于JVM运行时数据区的部分，也会被频繁地使用 ，也存在 OOM

## 二、HotSpot 对象
> Java 堆中对象分配、布局和访问

### 2.1 Java创建对象的过程
etc
* 类检查检查：JVM 遇到 new 指令时，先检查常量池中是否能定位到这个类的符号引用，并检查这个类是否已被加载、解析和初始化过
* 分配内存：为新生对象分配内存。分配方式：“指针碰撞” 和 “空闲列表”两种。选择哪种分配方式取决于 Java 堆是否规整，而 Java 堆是否规整又由垃圾收集器是否带有压缩整理的功能
  * 内存分配并发问题：
    * CAS+失败重试：CAS 是乐观锁的一种实现方式。保证更新操作的原子性
    * TLAB：为每个线程在 Eden 区预留一块内存，首先在 TLAB 分配，当对象大于 TLAB 中剩余内存或 TLAB 内存用尽时，再使用上面 CAS 进行内存分配
* 初始化零值：将分配到的内存空间都初始化为零值，保证对象的实例字段能被直接使用
* 设置对象头：对对象进行必要设置，如对象所属类、类的元数据信息地址、对象的哈希吗、对象的 GC 分代年龄等
* 执行 init 方法：初始化对象数据

### 2.2 对象的内存布局
> 对象在内存中的布局：对象头、实例数据、对齐填充

* 对象头：对象运行时数据（哈希吗，GC 分代年龄，锁状态标志） + 类型指针（类元数据）
* 实例数据：对象真实数据
* 对齐填充：占位，HotSpot 虚拟机的自动内存管理系统要求对象起始地址必须是 8 字节的整数倍

### 2.3 对象的访问定位
> 调用对象数据，访问方式由虚拟机实现而定：1 使用句柄，2 直接指针

* 句柄：Java 堆中划分出一块内存“句柄池”，栈桢中的引用存储的就是对象的句柄地址，而句柄中包括了对象实例数据与是类型数据各自的具体地址信息

**优点**
句柄地址稳定，对西那个被移动时只会改变句柄中的实例数据指针

**对象的访问定位-使用句柄**
etc

* 直接指针：reference 存储对象的地址

**优点**
速度快

**对象的访问定位-直接指针**
etc

## 三、String 类和常量池

### 3.1 String 对象两种创建方式
```java
String str1 = "abcd";//先检查字符串常量池中有没有"abcd"，如果字符串常量池中没有，则创建一个，然后 str1 指向字符串常量池中的对象，如果有，则直接将 str1 指向"abcd""；
String str2 = new String("abcd");//堆中创建一个新的对象
String str3 = new String("abcd");//堆中创建一个新的对象
System.out.println(str1==str2);//false
System.out.println(str2==str3);//false
```
**string对象**
etc

Q：String str = new String("abc") 创建对象个数
A：1 或 2，如果常量池中有字符串常量“abc”，则只在堆中创建一个对象，反之则创建两个，先在常量池中创建，再在堆上创建

### 3.2 基本类型、包装类型、常量池
> Byte,Short,Integer,Long,Character,Boolean

**性能与资源之间的权衡**
* Byte,Short,Integer,Long 默认创建了数据[-128,127] 的相关类型的缓存数据
* Character 创建了数值在 [0.127] 范围的缓存数据，Boolean 直接返回创建好的 True or False

```java
Integer i1 = 40; // Integer i1 = Integer.valueOf(40);
Integer i2 = new Integer(40);  // 新对象
```

## 四、垃圾回收
> Java 自动内存管理主要针对：对象内存的回收和分配
etc

`Java 堆是垃圾收集器管理的主要区域，又名 GC 堆。由于现代收集器基本采用分代垃圾收集算法，进一步划分的目的是更好地回收内存和更快的分配内存`
* Java 堆
  * 新生代
    * Eden 区:大部分对象分配区域，在新生代垃圾回收后，如果对象还存活，则进入 s0 或 s1，对象年龄+1
    * From Survivor 区：from & to 在 GC 后交换角色
    * To Survivor 区：每次都是空的
  * 老年代：对象年龄到 JVM 规定的值后晋升到老年代，通过 -XX:MaxTenuringThreshold 设置，这个值在 JVM 运行过程中会动态调整

### 4.1 堆空间基本结构
etc 

```properties
-verbose:gc
-Xmx200M
-Xms200M
-Xmn50M
-XX:+PrintGCDetails
-XX:TargetSurvivorRatio=60
-XX:+PrintTenuringDistribution
-XX:+PrintGCDetails
-XX:+PrintGCDateStamps
-XX:MaxTenuringThreshold=3
-XX:+UseConcMarkSweepGC
-XX:+UseParNewGC

```

### 4.2 堆内存分配策略
etc
* 对象优先分配在 Eden 区
* 大对象直接进入老年代：避免`担保机制`带来的复制而降低效率（确保 Minor GC 前，老年代剩余空间本身能容纳新生代所有对象）
* 长期存活的对象将进入老年代：年龄到一定程度晋升到老年代（默认15岁）

HotSopt JM 两类 GC
`部分收集（Partial GC）`
* 新生代手机（Minor GC/Young GC）：只对新生代进行垃圾收集
* 老年代收集（Majoy GC/Old GC）：只对老年代
* 混合收集（Mixed GC）：对新生代和老年代收集
`整堆手机（Full GC）`：收集整个 Java 堆和方法区

### 4.3 对象死亡
etc
* 引用计数器法：简单、效率高，主流虚拟机中没有被使用。无法解决循环引用问题
* 可达性分析算法：通过一些列“GC r、Roots”作为起点，从这些起点向下搜索，当一个对象到 GC Roots 没有任何引用链，则对象不可用

etc

`可作为 GC Roots 的对象包括以下：`
1. 虚拟机栈（栈帧中的本地变量表）中引用的对象（方法中变量）
```java
// a 是栈帧中的本地变量，a 就是 GC Root，由于 a = null，a 与 new Rumenz() 对象断开链接，所以对象会被回收
public class Rumenz{
    public static  void main(String[] args) {
         Rumenz a = new Rumenz();
         a = null;
    }
}
```
2. 本地方法栈（Native 方法）中引用的对象
3. 方法区中类静态属性引用的对象（类 static 属性）
```java
// 栈帧中的本地变量a=null,由于a断开了与GC Root对象(a对象)的联系,所以a对象会被回收。由于给Rumenz的成员变量r赋值了变量的引用,并且r成员变量是静态的,所
// 以r就是一个GC Root对象,所以r指向的对象不会被回收
public class Rumenz{
    public static Rumenz r;
    public static void main(String[] args){
       Rumenz a=new Rumenz();
       a.r=new Rumenz();
       a=null;
    }
}
```
4. 方法区中常量引用的对象（类 final 属性）
```java
// 常量r引用的对象不会因为a引用的对象的回收而被回收
public class Rumenz{
    public static final Rumenz r=new Rumenz();
    public static void main(String[] args){
       Rumenz a=new Rumenz();
       a=null;
    }
  
}
```
5. 所有被同步锁持有的对象（synchronized 对象）

## 五、引用
1. 强引用：必不可少，即使 OOM 也不会被回收
2. 软引用：可有可无，内存空间不足才会被回收。可用做实现内存敏感的高速缓存
3. 弱引用：可有可无，生命周期更短，垃圾回收器线程扫描内存区域，进行下一次 GC 时就会被回收，但垃圾回收器线程优先级很低，不一定很快发现弱引用对象
4. 虚引用：任何时候都可能被回收，用来跟踪对象被回收的活动

* 不可达对象并非“非死不可”：需要经过二次标记，可达性分析会产生一次标记，然后判断此对象是否有必要执行 finalize 方法，当对象没有覆盖 finalize 方法或者 finalize 方法已经被虚拟机调用过时则为没必要执行。若对象在第二次标记前与其他对象产生引用链，则不会被回收
* 废弃常量判断：无 String 对象引用的字符串常量
* 无用的类
  * 所有实例被回收
  * 加载该类的 ClassLoader 已经被回收
  * 该类的 Class 对象无引用，无法通过反射访问该类的方法


## 六、垃圾收集算法
etc

### 6.1 标记-清除算法
etc
**步骤**
1. 标记：标记所有不需要回收的对象
2. 清除：清理未标记对象
**存在问题**
1. 效率问题
2. 空间问题（大量碎片）

### 6.2 标记-复制算法
> 将内存大小分为大小相同的两块，每次使用其中一块，每次一块内存使用完后将存活的对象复制到另一块中，然后把之前一块一次性清理
etc

### 6.3 标记-整理算法
* 标记
* 移动（向一端移动）
* 清除（边界线以外的内存）
etc

### 6.4 分代收集算法
> 根据对象存活周期的不同将内存分为几块，根据各年代的特点选择合适的垃圾收集算法


## 七、垃圾收集器
> 垃圾回收算法是内存回收的方法论，垃圾收集器就是内存回收的具体实现
etc

### 7.1 Serial 收集器
> 单线程收集，工作时必须暂停其他所有的工作线程，直到收集结束。无线程切换的开销
* 新生代：标记-复制算法
* 老年代：标记-整理算法
etc

### 7.2 ParNew 收集器
> Serial 收集器多线程版本，除了使用多线程进行垃圾收集外，其他行为和 Serial 完全一样

etc
* 新生代：标记-复制算法
* 老年代：标记-整理算法

### 7.3 Parallel Scavenge 收集器（JDK8 默认收集器：Parallel Scavenge + Parallel Old）
> 和 ParNew 几乎一样，关注点是吞吐量（高效的利用 CPU）

etc

### 7.4 Serial Old 收集器
> Serial 收集器的老年代版本

### 7.5 Parallel Old 收集器
> Parallel Scavenge 收集器的到年代版本

### 7.6 CMS 收集器（Concurrent Mark Sweep）
> 以获取最短回收停顿时间为目的的收集器，注重用户体验。HotSpot 虚拟机第一款真正意义上的并发收集器

**清除步骤**
1. 初始标记：暂停所有的其他线程，记录直接与 GC Roots 相连的对象，这一步很快
2. 并发标记：同时开启 GC 和用户线程，记录可达对象（不保证实时性，因为用户线程会更新引用域）
3. 重新标记：修正并发标记期间因为用户线程持续运行而导致标记误差的部分对象记录
4. 并发标记：开启用户线程，GC 线程开始对未标记区域清理

etc

**优点：** 并发收集、低停顿
**缺点：** 
* 对 CPU 资源敏感
* 无法处理浮动垃圾
* 产生空间碎片

### 7.7 G1 收集器
> 面向服务器的垃圾收集器，针对配备多颗处理器及大容量内存的机器，同时满足 GC 停顿时间要求和高吞吐量性能

**特点**
1. 并行与并发：G1 充分利用 CPU、多核环境下的硬件优势
2. 分代收集：
3. 空间整合：
4. 可预测的停顿：G1 追求低停顿同时还能预测停顿时间

**步骤**
* 初始标记
* 并发标记
* 最终标记
* 筛选回收

`G1 收集器在后台维护了一个优先列表，每次根据允许的收集时间，优先选择回收价值最大的 Region`

### 7.8 ZGC 收集器