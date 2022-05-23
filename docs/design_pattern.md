`设计模式 Java`
[原文地址](https://www.bookstack.cn/books/design-pattern-java)

- [一、入门](#一入门)
  - [1.1. 概述](#11-概述)
    - [1.1.1 招式 VS 内功(一)](#111-招式-vs-内功一)
    - [1.1.2 招式 VS 内功(二)](#112-招式-vs-内功二)
    - [1.1.3 招式 VS 内功(三)](#113-招式-vs-内功三)
  - [1.2 面向对象设计原则](#12-面向对象设计原则)
    - [1.2.1 面向对象设计原则-单一职责原则](#121-面向对象设计原则-单一职责原则)
    - [1.2.2 面向对象设计原则-开闭原则](#122-面向对象设计原则-开闭原则)
    - [1.2.3 面向对象设计原则-里氏替换原则](#123-面向对象设计原则-里氏替换原则)
    - [1.2.4 面向对象设计原则-依赖倒转原则](#124-面向对象设计原则-依赖倒转原则)
    - [1.2.5 面向对象设计原则-接口隔离原则](#125-面向对象设计原则-接口隔离原则)
    - [1.2.6 面向对象设计原则-合成复用原则](#126-面向对象设计原则-合成复用原则)
    - [1.2.7 面向对象设计原则-迪米特法则](#127-面向对象设计原则-迪米特法则)
- [二、创建型模式](#二创建型模式)
  - [2.1 简单工厂模式](#21-简单工厂模式)
    - [2.1.1 简单工厂模式（一）](#211-简单工厂模式一)
    - [2.1.2 简单工厂模式（二）](#212-简单工厂模式二)
  - [2.2 工厂方法模式](#22-工厂方法模式)
    - [2.2.1 工厂方法模式（一）](#221-工厂方法模式一)
  - [2.3 抽象工厂模式](#23-抽象工厂模式)
  - [2.3.1 抽象工厂模式（一）](#231-抽象工厂模式一)
  - [2.4.1 单例模式](#241-单例模式)
  - [2.4.1 单例模式（一）](#241-单例模式一)
  - [2.5 原型模式](#25-原型模式)
    - [2.5.1 原型模式（一）](#251-原型模式一)
  - [2.6 建造者模式](#26-建造者模式)
- [三、结构型模式](#三结构型模式)
  - [3.1 适配器模式](#31-适配器模式)
  - [3.2 桥接模式](#32-桥接模式)
  - [3.3 组合模式](#33-组合模式)
  - [3.4 装饰器模式](#34-装饰器模式)
  - [3.5 外观（门面）模式](#35-外观门面模式)
  - [3.6 享元模式](#36-享元模式)
  - [3.7 代理模式](#37-代理模式)
- [四、行为型模式](#四行为型模式)
  - [4.1 责任链模式](#41-责任链模式)
  - [4.2 命令模式](#42-命令模式)
  - [4.3 解释器模式](#43-解释器模式)

---

# 一、入门

## 1.1. 概述

### 1.1.1 招式 VS 内功(一)

> "内功"大增，再结合日益纯熟的"招式"

招式：Java、C#、C ++、IDEA、JSP、MySQL等
内功：数据结构、算法、设计模式、重构、软件工程

`设计模式起源`
模式起源于建筑领域
大量调查研究和资料手机，发现人们对舒适住宅和城市环境存在一些共同`认同规律`
把这些`认同规律`归纳为253个模式，从前提条件、目标问题、解决方案三个方面进行描述，给出用户需求到建筑环境结构设计直至经典实例的过程模型

`模式是在特定环境下人们解决某类重复问题的一套成功或者有效的解决方案`

GoF 归纳发表了23种在软件开发中使用频率较高的设计模式，用于统一沟通面向对象方法在分析、设计和实现间的鸿沟

软件模式基础结构：问题描述、前提条件、解法和效果

### 1.1.2 招式 VS 内功(二)

> 通过一些成熟的设计方案来指导新项目的开发和设计，便于开发出具有更好的灵活性和可扩展性的软件系统
> 一般定义：设计模式是一套被反复使用、多数人知晓的、经过分类编目的、代码设计经验的总结，使用设计模式是为了可重用代码、让代码更容易被他人理解并且保证代码可靠性

根据用途，设计模式可分为创建型、结构型、行为型三种

* 创建型（5种）：用于描述如何创建对象
* 结构型（7种）：用于描述如何实现类或对象的组合
* 行为型（11种）：用于描述类或对象怎样交互以及怎样分配职责

`常用设计模式一览表-学习难度-使用频率`

![design-pattern-java](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/design-pattern-java.png)

### 1.1.3 招式 VS 内功(三)

`用途`

1. 实现可维护性复用的设计方案
2. 提供一套通用的设计词汇和形式，方便开发人员之间的沟通，减少结构成本，因为设计模式是跨语言、跨平台、跨应用、跨国界的
3. 兼顾系统可重用性和可扩展性，更好地重用已有的设计方案、功能模块，避免重复造轮子。
4. 便于他人理解你的设计思路和实现方案

`重点`

1. 能掌握，关键在于多思考、多实践
2. 了解每一种设计模式的意图，指定使用这个设计模式的使用场景以及使用该模式的优势
3. 在实际开发种灵活运用
4. 不可滥用，每个模式都有自己的适用场景，不能为了使用模式而使用模式
5. 不管这个设计模式难度以及使用频率如何，都应好好学学，每一个模式都是一种计策，为解决某一类问题二诞生
6. 实际开发中设计模式信手拈来

## 1.2 面向对象设计原则

> 在支持可维护性的同时，提高系统可复用性是一个至关重要的问题
> 面向对象设计原则为支持可维护性可复用性而诞生

![7种面向对象设计原则](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/origin-object-7-principle.png)

### 1.2.1 面向对象设计原则-单一职责原则

> 一个类只负责一个功能领域种的相应职责，或者可以定义为：就一个类而言，应该只有一个引起它变化的原因
> 一个类（大到模块，小到方法）承担的职责越多，它被复用的可能性就越小

`实例：`
需求：CRM系统中客户信息图形统计模块设计方案

初始设计方案结构图：

![单一职责-Source](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/principal-single-responsibilities-source.jpg)

`描述：`

1. getConnection()  用于连接数据库
2. findCustomers() 用于查询所有客户信息
3. createChart()和displayChart() 分别用于创建和显示图表

`重构`
使用单一职责原则对其进行重构

![单一职责-Target](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/principal-single-responsibilities-target.jpg)

`描述：`

1. DBUtil：负责连接数据库，其中包含获取数据连接的方法
2. CustomerDAO：负责操作数据库中customer表，其中可以包含CURD方法
3. CustomerDataChart：负责图表的创建和显示

### 1.2.2 面向对象设计原则-开闭原则

> 最重要。
> 一个软件实体应当对扩展开放，对修改关闭。即软件实体尽量在不修改原有的代码情况下进行扩展
> 软件实体：一个软件模块、一个由多个类组成的局部结构或者要给独立的类

任何软件的需求都会随着时间的推移而发生变化。当系统需要面对新的需求时，我们一共尽量保证系统的设计框架是稳定的。
使得系统在拥有适应性和灵活性的同时具备良好的稳定性和延续性

`抽象化是开闭原则的关键`

`实例`
需求：CRM系统需要可以显示各种类型的图表，如饼状图和柱状图等，为了支持多种图表显示方式，原始设计方案如图

![开闭原则-原始](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/principal-open-close-source.jpg)

ChartDisplay 类中的display() 方法代码片段如下：

```java
......  
if (type.equals("pie")) {  
    PieChart chart = new PieChart();  
    chart.display();  
}  
else if (type.equals("bar")) {  
    BarChart chart = new BarChart();  
    chart.display();  
}  
......
```

`描述`

在该代码中，如果需要增加一个新的图表类，如折线图LineChart，则需要修改ChartDisplay类中的display() 方法的源代码，增加新的判断逻辑，违背了开闭原则

`重构`

现对该系统进行重构，可以通过抽象化的方式对系统进行重构，使之增加新的图表类时无须修改源代码

具体做法如下：

1. 增加要给抽象图表类 AbstractChart，作为各种具体图表类的基类
2. ChartDisplay类针对抽象图表类进行编程，由客户端决定具体使用哪种图表

![开闭原则-重构](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/principal-single-responsibilities-target.jpg)

`描述`

1. 引入抽象图表类AbstractChart，ChartDisplay针对抽象图表类进行编程，并通过setChart()方法由客户端来设置具体图表对象
2. 如果需要增加一种新的图表，如折线图LineChart，则只需将LineChart继承AbstractChart即可

### 1.2.3 面向对象设计原则-里氏替换原则

> 所有引用基类（父类）的地方必须能透明使用其子类的对象

在软件中将基类对象替换成它的子类对象，程序将不产生任何错误和异常，反之则不行

如父类 BaseClass，子类 SubClass
任意一个方法，如果可以接受一个 BaseClass类型的对象的话，那么这个方法一定能接受一个 SubClass 的对象

`注意事项`

1. 子类所有的方法必须在父类中声明
2. 尽量把父类设计为抽象类或者接口，让子类继承父类或者实现父接口，并在父类中声明方法。里氏替换原则是开闭原则的具体实现手段之一
3. Java中，在编译阶段，Java编译器会检查程序是否符合里氏替换原则

`实例`

需求：客户可以分为VIP客户和普通客户两类，系统需要提供一个发送Email功能，原始设计方案如下

![里氏替换-原始](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/lsp-principal-source.jpg)

`描述`

1. 无论是普通用户还是VIP用户，发送邮件的过程都是相同的，即send()方法中代码重复
2. 如果新增其他类型用户，扩展性差

`重构`

`描述`

1. 考虑增加一个抽象客户类 Customer
2. CommonCustomer和VIPCustomer 类作为抽象客户类的子类
3. 邮件发送类EmailSender 针对抽象客户类Customer编程，根据里氏替换原则，能够接受基类对象的方法必然能够接受子类对象

![里氏替换-重构](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/lsp-principal-target.jpg)

`注意`

里氏替换原则是实现开闭原则的重要方式之一。除了在传递参数使用基类对象时，在定义成员变量、定义局部变量、确定方法返回类型时都可以使用里氏替换原则

### 1.2.4 面向对象设计原则-依赖倒转原则

> 抽象不应该依赖于细节，细节应当依赖于抽象

依赖倒转原则要求我们在传递参数时或在关联关系中，尽量引用层次高的抽象类，即使用接口和抽象类进行变量类型声明、参数类型声明、方法返回类型声明，不要用具体的类来做这些事情。

实现依赖倒转原则时，需要针对抽象层编程，而将具体类的对象通过依赖注入的方式注入到其他对象中，依赖注入是指当要给对象要与其他对象发生依赖关系时，通过抽象来注入所依赖的对象。

常用的注入方式：构造注入、设值注入（Setter）和接口注入

`实例`

需求：在某CRm中，系统经常需要将存储在TXT或者Excel文件中的客户信息转存到数据库中，因此需要进行数据格式转换

原始设计方案结构图:

![依赖注入-原始](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/principal-di-source.jpg)

`存在问题：`

1. 每次转换数据时数据来源不一定相同，因此需要更换数据转换类，如有时候需要将TXTDataConvertor改为ExcelDataConvertor，此时需要修改CustomerDAO  源代码
2. 引入并使用新的数据转换类时也不得不改CustomerDAO源代码
3. 扩展性差，违背开闭原则

`重构`

`描述`

1. 引入抽象数据转换类DataConvertor，CustomerDAO针对抽象类DataConvertor编程
2. 将具体的转换类名存储在配置文件中，符合依赖倒转原则
3. 根据里氏替换原则，程序在运行时，具体数据转换类对象将替换DataConvertor类型的对象
4. 引入新的数据转换类时无需修改源代码，只需修改配置文件

![依赖注入-重构](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/principal-di-target.jpg)

`上述重构中，使用了开闭原则、里氏替换原则和依赖倒转原则，大多数情况下，这三个设计原则会同时出现，开闭原则时目标，里氏替换原则时基础，依赖倒转原则是手段，相辅相成，相互补充，目标一致`

### 1.2.5 面向对象设计原则-接口隔离原则

> 使用多个功能单一的接口，而不使用单一的总接口，即客户端不应该依赖那些它不需要的接口
> 当一个接口太大时，需要将它分割成多个更小的接口，使得实现该接口的子类仅需知道与之相关的方法即可

`实例`

需求：某CRM系统的客户数据显示模块设计如图所示

![接口隔离-原始](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/isp-principal-source.jpg)

`描述`

1. dataRead()  用于从文件中读取数据
2. transformToXML() 用于将数据转换成 XMl 格式
3. createChart()/display() 用于创建图表和显示图表
4. createReport()/displayReport() 用于创建文字报表和显示文字报表

`存在问题：`

1. 接口庞大，不灵活。
2. 部分功能只需其中部分方法，但每次都需要实现该接口中所有的方法，代码冗余

`重构`

![接口隔离-重构](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/isp-principal-target.jpg)

`描述`

1. 使用接口隔离原则时，注意控制接口粒度，不能太大或者太小

### 1.2.6 面向对象设计原则-合成复用原则

> 尽量使用对象组合，和不是继承来达成复用的目的
> 在新的对象里通过关联关系（包括组合关系和聚合关系）来使用一些已有的对象，使之成为新对象的一部分
> 在面向对象设计中，通过组合/聚合或者通过继承来复用已有的设计和实现
> 组合/聚合使系统更加灵活，降低类之间耦合度，适当使用继承有助于对问题的理解，降低复杂度，而滥用继承则会更加系统构建和维护的难度以及系统的复杂度

`如何抉择组合/聚合和继承`

* Has-A：使用组合/聚合
* Is-A：使用继承

`实例`

描述：CRM系统设计初期，考虑客户数量不多，采用MySQL作为数据库，与数据库操作相关的类如 CustomerDAO 都需要连接数据库，而连接数据库的方法被封装在DBUtil中，由于需要重用DBUtil类的getConenction()方法，设计人员将CustomerDAO作为DBUtil的子类

![合成复用-原始](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/principal-carp-source.jpg)

`问题描述`

1. 随着客户数量增加，系统决定使用Oracle数据库，因此需要新增要给OracleDbUtil类来连接Oracle数据库
2. 由于初始设计方案中CustomerDAO和DBUtil之间是继承关系，因此更换数据库连接方式时需要修改CustomerDAO的源代码，使其继承OracleDBUtil类
3. 违背开闭原则

`重构`

`描述`

1. 根据合成复用原则，在实现复用时，少用继承，多用关联
2. CustomerDAO和DBUtil之间的关系由继承变为关联，采用依赖注入方式将DBUtil对象注入CustomerDAO中
3. 如果需要对DBUtil的功能进行扩展，可以通过其子类来实现，如MySQLDBUtil和OracleDBUtil类

![合成复用-重构](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/principal-carp-target.jpg)

### 1.2.7 面向对象设计原则-迪米特法则

> 又称最少知道原则。一个软件实体应当尽可能少地与其他实体发生相互作用
> 当一个模块发生修改时，会尽量少地影响其他模块，扩展相对容易，且降低系统耦合度，使类之间保持松散的耦合关系

迪米特法则：不要和"陌生人"说话，只与"朋友"通信

`朋友`

1. 当前对象本身（this）
2. 以参数形式传入当前对象方法中的对象
3. 当前对象的成员对象（如果成员对象是一个集合，则集合中的元素也是）
4. 当前对象所创建的对象

`实例`

需求：某CRM系统包含很多业务操作窗口，这些窗口中，某些界面控件间存在复杂的交互关系，一个控件事件的触发将导致多个其他界面控件产生响应。如当一个按钮被点击时，对应的列表框、组合框、文本框、文本标签等都将发生改变

`描述`

1. 界面控件之间交互关系复杂
2. 在该窗口中增加新的界面控件时需要修改与其交互的其他控件的源代码，系统扩展性差，也不便于增删控件

![迪米特法则-原始](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/principal-lkp-source.jpg)

`重构`

`描述`

1. 引入一个专门用于控制界面控件交互的中间类(Mediator) 来降低界面控件之间的耦合度
2. 引入中间类后界面控件之间不再发生直接引用，而是先将请求转发给中间类，在有中间类完成对其他控件的调用
3. 当需要增加或者删除控件时，只需修改中间类即可

![迪米特法则-重构](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/principal-lkp-target.jpg)

# 二、创建型模式

> 六个。
> 简单工厂模式
> 工厂方法模式
> 抽象工厂模式
> 单例模式
> 原型模式
> 建造者模式

## 2.1 简单工厂模式

### 2.1.1 简单工厂模式（一）

> 工厂模式时最常用的一类创建型设计模式，即工厂方法模式
> 简单工厂模式是工厂方法模式的"小弟"，抽象工厂模式是工厂方法模式的"大哥"

`实例`
需求：开发一套图表库，可以为应用系统提供各种不同外观的图表，如柱状图、饼状图、折线图等。希望提供一套灵活的图表库，且方便对图表库进行扩展，便于将来增加新类型的图表

```java
/**
 *  @Description 图表
 *  
 *  @author wudiguang
 *  @Date 2021/11/28
 */ 
public class Chart {
    /*图表类型*/
    private String type;

    /*根据不同type初始化不同的图表*/
    public Chart(Object[][] data, String type){
        this.type = type;
        if(Objects.equals("histogram", type)){
            // 初始化饼状图
        }else if(Objects.equals("pie", type)){
            // 初始化饼状图
        }else if(Objects.equals("line", type)){
            // 初始化折线图
        }
    }

    /*显示*/
    public void display(){
        if(Objects.equals("histogram", this.type)){
            // 显示饼状图
        }else if(Objects.equals("pie", this.type)){
            // 显示饼状图
        }else if(Objects.equals("line", this.type)){
            // 显示折线图
        }
    }
}
```

客户端通过调用Chart类构造函数来创建图表对象，根据type不同，可以得到不同类型的图表，再调用display()来显示相应的图表

`存在问题`

1. Chart 中包含大量 "if...else..."，代码冗长，阅读、维护和测试困难。且大量条件语句将影响系统型能，系统需要做大量条件判断
2. Chart 类职责过重，它负责初始化和显示所有的图表对象，违背了"单一职责原则"，不利于类的重用和维护，且将大量的对象初始化代码都皆在了构造函数中，导致构造函数非常庞大，降低了对象的创建效率
3. 当需要增加新类型的图表时，必须修改Chart类，违背了"开闭原则"
4. 客户端只能通过new关键字直接创建Chart对象，Chart类与客户端类耦合度较高，对象的创建和使用无法分离
5. 客户端再创建Chart对象之前可能需要进行大量初始化设置，如颜色、大小等，如果Chart类中没有提供要给默认设置，则只能由客户端来完成，这些代码每次创建Chart对象时都会出现，导致代码大量重复

### 2.1.2 简单工厂模式（二）

> 增加几个角色：具体产品类、工厂类、抽象产品类
> 定义：定义一个工厂类，它可以根据参数的不同返回不同类的实例，被创建的实例通常都具有共同的父类，因为简单工厂模式中创建实例的方法是静态方法，因此简单工厂模式又被称为静态工厂方法模式

![简单工厂模式-原始](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/creative-simple-factory-pattern-source.png)

`完整解决方案`

![简单工厂模式-重构](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/creative-simple-factory-pattern-target.png)

```java
/**
 *  @Description 抽象图表接口：抽象产品类
 *  
 *  @author wudiguang
 *  @Date 2021/11/28
 */ 
public interface IChartService {
    /*显示*/
    void display();
}
```

```java
/**
 *  @Description 柱状图：具体产品类
 *  
 *  @author wudiguang
 *  @Date 2021/11/28
 */ 
public class HistogramChartServiceImpl implements IChartService{
    public HistogramChartServiceImpl() {
        System.out.println("创建柱状图~");
    }

    @Override
    public void display() {
        System.out.println("显示柱状图~");
    }
}
```

```java
/**
 *  @Description 饼状图：具体产品类
 *  
 *  @author wudiguang
 *  @Date 2021/11/28
 */ 
public class PieChartServiceImpl implements IChartService{
    public PieChartServiceImpl() {
        System.out.println("创建饼状图~");
    }

    @Override
    public void display() {
        System.out.println("显示饼状图~");
    }
}
```

```java
/**
 *  @Description 折线图：具体产品类
 *  
 *  @author wudiguang
 *  @Date 2021/11/28
 */ 
public class LineChartServiceImpl implements IChartService{
    public LineChartServiceImpl() {
        System.out.println("创建折线图~");
    }

    @Override
    public void display() {
        System.out.println("显示折线图~");
    }
}
```

```java
import java.util.Objects;

/**
 *  @Description 图表工厂类：工厂类
 *  
 *  @author wudiguang
 *  @Date 2021/11/28
 */ 
public class ChartFactory {
    /*静态工厂方法*/
    public static IChartService getChart(String type){
        IChartService chart = null;
        if(Objects.equals("histogram", type)){
            // 初始柱饼状图
            chart = new HistogramChartServiceImpl();
        }else if(Objects.equals("pie", type)){
            // 初始化饼状图
            chart = new PieChartServiceImpl();
        }else if(Objects.equals("line", type)){
            // 初始化折线图
            chart = new LineChartServiceImpl();
        }
        return chart;
    }
}
```

```java
/**
 *  @Description 客户端测试
 *
 *  @author wudiguang
 *  @Date 2021/11/28
 */
public class Client {
    public static void main(String[] args) {
        IChartService chart = ChartFactory.getChart("pie");
        chart.display();;
    }
}
```

`方案的改进`

可以将 type 参数的赋值放入 xml 配置中，符合"开闭原则"

`简单工厂模式简化`

将抽象产品类和工厂类合并，将静态工厂方法移至抽象产品类中

```java
/**
 *  @Description 抽象图表接口：抽象产品类
 *  
 *  @author wudiguang
 *  @Date 2021/11/28
 */ 
public abstract class AbstractChart {
    /*显示*/
    abstract void display();

    /*静态工厂方法*/
    public static AbstractChart getChart(String type){
        AbstractChart chart = null;
        if(Objects.equals("histogram", type)){
            // 初始柱饼状图
            chart = new HistogramChartServiceImpl();
        }else if(Objects.equals("pie", type)){
            // 初始化饼状图
            chart = new PieChartServiceImpl();
        }else if(Objects.equals("line", type)){
            // 初始化折线图
            chart = new LineChartServiceImpl();
        }
        return chart;
    }
}
```

`总结：`

> 提供了专门的工厂类用于创建对象，将对象的创建和使用分离开，作为一种最简单的工厂模式再软件开发中被广泛应用

**优点：**

1. 工厂类决定创建何种对象，客户端只负责消费
2. 客户端类无需和具体产品类交互，减少复杂性
3. 通过引入配置文件，可以满足"开闭原则"

**缺点：**

1. 工厂类集中创建所有具体产品逻辑，职责过重
2. 引入更多的类（工厂类），增加系统复杂性
3. 系统扩展困难，一旦增加新的产品将不得不修改工厂类逻辑
4. 由于工厂模式使用了静态工厂方法，导致工厂角色无法形成基于继承的结构

**使用场景：**

1. 工厂类负责创建的对象比较少
2. 客户端只知道传入工厂类的参数，对于如何创建对象并不关心

**练习：**

> 使用简单工厂模式设计一个可以创建不同几何形状（圆形、方形、三角形）的绘图工具，每个图形都具有绘制和擦除的方法

## 2.2 工厂方法模式

`实例`

描述：日志记录器的设计，该记录器可以通过多种途径保存系统的运行日志，如通过文件记录或者数据库记录，用于可以通过修改配置文件灵活地更换日志记录方式。

`面临问题：`

1. 需要对日志记录器进行一些初始化工作，初始化参数的设置过程较为复杂。且还可能需要读取配置文件（如连接数据库或创建文件）
2. 参数的设置由严格的先后次序，否则可能发生记录失败
3. 支持用于通过修改配置方式来切换日志记录方式

`使用简单工厂`

![使用简单工厂](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/creative-factory-method-pattern-source.png)

```java
/**
 *  @Description 日志记录器工厂(简化版)
 *  
 *  @author wudiguang
 *  @Date 2021/11/28
 */ 
public class LoggerFactory {
    /*静态工厂方法*/
    public static Logger createLogger(String type){
        if(Objects.equals("db", type)){
            // 连接数据库
            // 创建数据库日志记录器对象
            Logger logger = new DatabaseLogger();
            // 初始化数据库日志记录器，略
            return logger;
        }else if(Objects.equals("file", type)){
            // 连接日志文件
            // 创建文件日志记录器对象
            Logger logger = new FileLogger();
            // 初始化文件日志记录器，略
            return logger;
        }
        return null;
    }

    /*抽象日志类*/
    interface Logger{

    }
    /*数据库日志记录器*/
    public static class DatabaseLogger implements Logger{

    }
    /*文件日志记录器*/
    public static class FileLogger implements Logger{

    }
}
```

`存在问题`

1. 工厂类过于庞大，包含大量"if...else"代码，维护和测试难度增大
2. 系统扩展不灵活，如果增加新类型的日志记录器，则必须修改静态工厂方法的业务逻辑，违背了"开闭原则"

### 2.2.1 工厂方法模式（一）

> 角色：抽象产品、具体产品、抽象工厂、具体工厂
> 相比于简单工厂模式，工厂方法模式引入了抽象工厂角色，抽象工厂可以是接口，也可以是抽象类或者具体类

`完整解决方案`

![使用工厂方法模式](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/creative-factory-method-pattern-target.png)

```java
//日志记录器接口：抽象产品  
interface Logger {  
    public void writeLog();  
}  
//数据库日志记录器：具体产品  
class DatabaseLogger implements Logger {  
    public void writeLog() {  
        System.out.println("数据库日志记录。");  
    }  
}  
//文件日志记录器：具体产品  
class FileLogger implements Logger {  
    public void writeLog() {  
        System.out.println("文件日志记录。");  
    }  
}  
//日志记录器工厂接口：抽象工厂  
interface LoggerFactory {  
    public Logger createLogger();  
}  
//数据库日志记录器工厂类：具体工厂  
class DatabaseLoggerFactory implements LoggerFactory {  
    public Logger createLogger() {  
            //连接数据库，代码省略  
            //创建数据库日志记录器对象  
            Logger logger = new DatabaseLogger();   
            //初始化数据库日志记录器，代码省略  
            return logger;  
    }     
}  
//文件日志记录器工厂类：具体工厂  
class FileLoggerFactory implements LoggerFactory {  
    public Logger createLogger() {  
            //创建文件日志记录器对象  
            Logger logger = new FileLogger();   
            //创建文件，代码省略  
            return logger;  
    }     
}
```

客户端测试代码

```java
class Client {  
    public static void main(String args[]) {  
        LoggerFactory factory;  
        Logger logger;  
        factory = new FileLoggerFactory(); //可引入配置文件实现  
        logger = factory.createLogger();  
        logger.writeLog();  
    }  
}
```

`反射与配置文件`

为了让系统具有更好的灵活性和可扩展性，引入反射机制和配置文件
通过反射来创建工厂对象

`重载的工厂方法`

![重载的工厂方法](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/creative-factory-method-pattern-overload-target.png)

```java
interface LoggerFactory {  
    public Logger createLogger();  
    public Logger createLogger(String args);  
    public Logger createLogger(Object obj);  
}
```

```java
class DatabaseLoggerFactory implements LoggerFactory {  
    public Logger createLogger() {  
            //使用默认方式连接数据库，代码省略  
            Logger logger = new DatabaseLogger();   
            //初始化数据库日志记录器，代码省略  
            return logger;  
    }  
    public Logger createLogger(String args) {  
            //使用参数args作为连接字符串来连接数据库，代码省略  
            Logger logger = new DatabaseLogger();   
            //初始化数据库日志记录器，代码省略  
            return logger;  
    }     
    public Logger createLogger(Object obj) {  
            //使用封装在参数obj中的连接字符串来连接数据库，代码省略  
            Logger logger = new DatabaseLogger();   
            //使用封装在参数obj中的数据来初始化数据库日志记录器，代码省略  
            return logger;  
    }     
}  
//其他具体工厂类代码省略
```

`工厂方法的隐藏`

![工厂方法的隐藏](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/creative-factory-method-pattern-hidden-source.png)

```java
//改为抽象类  
abstract class LoggerFactory {  
    //在工厂类中直接调用日志记录器类的业务方法writeLog()  
    public void writeLog() {  
        Logger logger = this.createLogger();  
        logger.writeLog();  
    }  
    public abstract Logger createLogger();    
}
```

```java
class Client {  
    public static void main(String args[]) {  
        LoggerFactory factory;  
        factory = (LoggerFactory)XMLUtil.getBean();  
        factory.writeLog(); //直接使用工厂对象来调用产品对象的业务方法  
    }  
}
```

`总结`

**优点：**

1. 创建客户所需产品的同时隐藏了具体产品类的细节
2. 工厂自主确定创建何种产品对象并将细节完全封装在具体的工厂内部
3. 系统中加入新产品时，无需修改抽象工厂和抽象产品提供的接口，无需修改客户端，完全符合"开闭原则"

**缺点：**

1. 增加新产品时，需要编写新的具体产品类和具体工厂类，一定程度上增加了系统复杂度
2. 引入抽象层，增加了系统的抽象性和理解难度

**适用场景**

1. 客户端不知道它所需的对象的类
2. 抽象工厂类通过其子类来指定创建哪个对象

2021/11/29 19:22

## 2.3 抽象工厂模式

> 抽象工厂模式引入"产品族"，减少由工厂方法模式引入的大量的工厂类

## 2.3.1 抽象工厂模式（一）

`需求`

描述：开发一套界面皮肤库，可以对Java桌面软件进行界面优化。用户在使用时，可以通过菜单来选择皮肤，不同的皮肤将提供不同视觉效果的按钮、文本框、组合框等，其结构示意图如下：

![界面皮肤结构图](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/java-desk-skin-map.png)

该皮肤库需要具备良好的灵活性和可扩展性，用户可以自由选择不同的皮肤，开发人员可以在不修改既有代码基础上新增皮肤

使用工厂方法模式，初始结构图如下：

![工厂方法模式](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/java-desk-skin-map-source.png)

`存在问题：`

1. 新增了大量类，每一个具体的组件都需要新增一个具体工厂
2. 由于同一种风格的界面组件通常要一起显示，用户使用时需要逐个设置，十分复杂

`产品等级结构`：即产品的继承结构。如抽象类时电视机，子类时海尔电视机、海信电视机、TCL电视机
`产品族`：在抽象工厂模式中，产品族指由同一个工厂生产，位于不同产品等级结构中的一组产品，如海尔电器工厂生产的海尔电视及、海尔冰箱

![产品族与产品等级结构示意图](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/product-hiearachy.png)

`抽象工厂模式示意图`

![抽象工厂模式示意图](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/abstract-factory-map.png)

`抽象工厂模式结构图`

![抽象工厂模式结构图](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/pattern-abstract-factory-target.png)

`完整解决方案`

![完整解决方案](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/pattern-java-desk-skin-map-target.png)

```java
//在本实例中我们对代码进行了大量简化，实际使用时，界面组件的初始化代码较为复杂，还需要使用JDK中一些已有类，为了突出核心代码，在此只提供框架代码和演示输出。  
//按钮接口：抽象产品  
interface Button {  
    public void display();  
}  
//Spring按钮类：具体产品  
class SpringButton implements Button {  
    public void display() {  
        System.out.println("显示浅绿色按钮。");  
    }  
}  
//Summer按钮类：具体产品  
class SummerButton implements Button {  
    public void display() {  
        System.out.println("显示浅蓝色按钮。");  
    }     
}  
//文本框接口：抽象产品  
interface TextField {  
    public void display();  
}  
//Spring文本框类：具体产品  
class SpringTextField implements TextField {  
    public void display() {  
        System.out.println("显示绿色边框文本框。");  
    }  
}  
//Summer文本框类：具体产品  
class SummerTextField implements TextField {  
    public void display() {  
        System.out.println("显示蓝色边框文本框。");  
    }     
}  
//组合框接口：抽象产品  
interface ComboBox {  
    public void display();  
}  
//Spring组合框类：具体产品  
class SpringComboBox implements ComboBox {  
    public void display() {  
        System.out.println("显示绿色边框组合框。");  
    }  
}  
//Summer组合框类：具体产品  
class SummerComboBox implements ComboBox {  
    public void display() {  
        System.out.println("显示蓝色边框组合框。");  
    }     
}  
//界面皮肤工厂接口：抽象工厂  
interface SkinFactory {  
    public Button createButton();  
    public TextField createTextField();  
    public ComboBox createComboBox();  
}  
//Spring皮肤工厂：具体工厂  
class SpringSkinFactory implements SkinFactory {  
    public Button createButton() {  
        return new SpringButton();  
    }  
    public TextField createTextField() {  
        return new SpringTextField();  
    }  
    public ComboBox createComboBox() {  
        return new SpringComboBox();  
    }  
}  
//Summer皮肤工厂：具体工厂  
class SummerSkinFactory implements SkinFactory {  
    public Button createButton() {  
        return new SummerButton();  
    }  
    public TextField createTextField() {  
        return new SummerTextField();  
    }  
    public ComboBox createComboBox() {  
        return new SummerComboBox();  
    }  
}
```

`抽象工厂模式总结`

**优点**

1. 同工厂方法模式
2. 新增产品族很方便

**缺点**

1. 新增产品等级很麻烦

## 2.4.1 单例模式

> 确保创建实例唯一性
> 例子：Window 任务管理器

## 2.4.1 单例模式（一）

`核心原理`

1. 私有化构造方法
2. 类内部定义私有静态实例
3. 暴露内部定义的静态实例

![单例模式](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/pattern-singleton.png)

由于 `singleton = new Singleton()` 非原子操作，当在并发环境下，可能创建多个不同的 `singleton` 实例

`饿汉式单例`

```java
class EagerSingleton {   
    private static final EagerSingleton instance = new EagerSingleton();   
    private EagerSingleton() { }   
    public static EagerSingleton getInstance() {  
        return instance;   
    }     
}
```

`懒汉式单例`

```java
class LazySingleton {   
    private static LazySingleton instance = null;   
    private LazySingleton() { }   
    // 每次调用getInstance()时都需要进行线程锁定判断，在多线程高并发访问环境中，将会导致系统性能大大降低
    synchronized public static LazySingleton getInstance() {   
        if (instance == null) {  
            instance = new LazySingleton();   
        }  
        return instance;   
    }  
}
```

`懒汉式单例-优化`

> 存在安全问题

```java
public static LazySingleton getInstance() {   
    if (instance == null) {  
        synchronized (LazySingleton.class) {  
            instance = new LazySingleton();   
        }  
    }  
    return instance;   
}
```

`双重检查锁`

> 使用双重检查锁时，需要将成员变量增加修饰符 volatile，保证多线程可见性
> 由于volatile关键字会屏蔽Java虚拟机所做的一些代码优化，可能会导致系统运行效率降低，因此即使使用双重检查锁定来实现单例模式也不是一种完美的实现方式。

```java
class LazySingleton {   
    private volatile static LazySingleton instance = null;   
    private LazySingleton() { }   
    public static LazySingleton getInstance() {   
        //第一重判断  
        if (instance == null) {  
            //锁定代码块  
            synchronized (LazySingleton.class) {  
                //第二重判断  
                if (instance == null) {  
                    instance = new LazySingleton(); //创建单例实例  
                }  
            }  
        }  
        return instance;   
    }  
}
```

`静态内部类-单例`

```java
//Initialization on Demand Holder  
class Singleton {  
    private Singleton() {  
    }  
    private static class HolderClass {  
            private final static Singleton instance = new Singleton();  
    }  
    public static Singleton getInstance() {  
        return HolderClass.instance;  
    }  
    public static void main(String args[]) {  
        Singleton s1, s2;   
            s1 = Singleton.getInstance();  
        s2 = Singleton.getInstance();  
        System.out.println(s1==s2);  
    }  
}
```

`优点`

1. 可控
2. 节约系统资源

`缺点`

1. 难以扩展
2. 单例类职责过重
3. 可能被自动垃圾回收技术回收，将导致单例对象状态丢失

## 2.5 原型模式

> 对象克隆
> 角色：抽象原型类、具体原型类、客户类

### 2.5.1 原型模式（一）

`实例`
描述：OA系统中写工作周报，提供导入周报模板

`通用克隆`

```java
class ConcretePrototype implements Prototype{
    private String  attr; //成员属性
    public void  setAttr(String attr){
        this.attr = attr;
    }
    public String  getAttr(){
        return this.attr;
    }
    public Prototype  clone(){ //克隆方法
        Prototype  prototype = new ConcretePrototype(); //创建新对象
        prototype.setAttr(this.attr);
        return prototype;
    }
}
```

`Java 版本克隆`

```java
class ConcretePrototype implements  Cloneable{
……
    public Prototype  clone(){
    　　Object object = null;
    　　try {
    　　　　　object = super.clone();
    　　} catch (CloneNotSupportedException exception) {
    　　　　　System.err.println("Not support cloneable");
    　　}
    　　return (Prototype )object;
    }
……
}
```

`深度克隆 VS 浅度克隆`

> 对于内部包含对象的对象克隆
> 深度克隆：内部对象不同
> 浅度克隆：内部对象地址相同

**通过序列化来从头实现对象的深克隆**

`实例`

```java
import java.util.*;
//抽象公文接口，也可定义为抽象类，提供clone()方法的实现，将业务方法声明为抽象方法
interface OfficialDocument extends  Cloneable{
       public  OfficialDocument clone();
       public  void display();
}
//可行性分析报告(Feasibility Analysis Report)类
class FAR implements OfficialDocument{
       public  OfficialDocument clone(){
              OfficialDocument  far = null;
              try{
                     far  = (OfficialDocument)super.clone();
              }
              catch(CloneNotSupportedException  e){
                      System.out.println("不支持复制！");
              }
              return  far;
       }
       public  void display(){
              System.out.println("《可行性分析报告》");
       }
}
//软件需求规格说明书(Software Requirements Specification)类
class SRS implements OfficialDocument{
       public  OfficialDocument clone(){
              OfficialDocument  srs = null;
              try{
                     srs  = (OfficialDocument)super.clone();
              }
              catch(CloneNotSupportedException  e){ 
                     System.out.println("不支持复制！");
              }
              return  srs;
       }
       public  void display(){
              System.out.println("《软件需求规格说明书》");
       }
}
//原型管理器（使用饿汉式单例实现）
class  PrototypeManager{
       //定义一个Hashtable，用于存储原型对象
       private Hashtable ht=new Hashtable();
       private static PrototypeManager pm =  new PrototypeManager();
       //为Hashtable增加公文对象   
     private  PrototypeManager(){
              ht.put("far",new  FAR());
              ht.put("srs",new  SRS());               
     }
     //增加新的公文对象
       public void addOfficialDocument(String  key,OfficialDocument doc){
              ht.put(key,doc);
       }
       //通过浅克隆获取新的公文对象
       public OfficialDocument  getOfficialDocument(String key){
              return  ((OfficialDocument)ht.get(key)).clone();
       }
       public static PrototypeManager  getPrototypeManager(){
              return pm;
       }
}
```

## 2.6 建造者模式

> 将整体的部分组装完整后返回给用户

# 三、结构型模式
## 3.1 适配器模式

## 3.2 桥接模式

## 3.3 组合模式

## 3.4 装饰器模式
> 如：静态代理

## 3.5 外观（门面）模式
> 如 nginx 反向代理，隐藏各子系统内部细节


## 3.6 享元模式
> 如 Java 中字符串常量池
> 运用共享就似乎有效地支持大量细粒度对象的复用。系统只使用少量的对象，可以实现对象的复用
> 享元模式结构较为复杂，一般结合工厂模式一起使用
> 可以极大减少内存中对象的数量，使得相同或相似对象在内存中只保存一份，从而可以节约系统资源，提高系统性能。

## 3.7 代理模式



# 四、行为型模式

## 4.1 责任链模式
> 例子：采购单的分级审批。主任 -> 副董事长 -> 董事长 -> 董事会
> 链式接口具体处理类


## 4.2 命令模式
> 通过开关控制各类电器
> 自定义功能键、快捷键
> 将一个请求封装为一个对象，从而让我们可用不同的请求对客户进行参数化。对请求排队或者记录请求日志，以及支持可撤销的操作
> 如：redis redo log和undo log


## 4.3 解释器模式#