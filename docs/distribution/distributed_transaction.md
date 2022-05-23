# 分布式事务

- [分布式事务](#分布式事务)
  - [一、事务](#一事务)
  - [二、分布式事务](#二分布式事务)
    - [2PC](#2pc)
    - [分布式数据库的 2PC 改进模型](#分布式数据库的-2pc-改进模型)
    - [XA 规范](#xa-规范)
    - [MySQL XA](#mysql-xa)
    - [3PC](#3pc)
    - [TCC](#tcc)
    - [Seata](#seata)

---

```md
author: WuDG
datetime： 23:40:40 星期二 2021年11月26日
position： 杭州
```


[原文地址](https://mp.weixin.qq.com/s?__biz=MzkxNTE3NjQ3MA==&mid=2247485728&idx=1&sn=f1ea6c37d5eb0d2a69315a08b0d1263b&source=41#wechat_redirect)

![分布式事务](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/distribute_transaction.png)

## 一、事务
> ACID 是严格意义上的定义，指事务的实现必须具备原子性、一致性、隔离性和持久性
> 然而，严格意义上的事务很难达到，常见的数据库就通过各种隔离级别来从中找到属于自己的平衡，隔离级别越高性能越低。因此不会遵循意义上的事务。

## 二、分布式事务
> 由于互联网快速发展，过去的单体架构难以支撑如今的流量。
> 且服务难以动态伸缩，因此进行服务拆解势在必行，由此产生微服务架构

![微服务框架](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/microservices.png)

每个服务能独立运行，独立部署
服务之间由本地调用变成远程调用，调用链路边长了，一次调用的耗时更长了，但是总体的吞吐量更大了

服务拆分引入其他问题：
1. 服务链路的监控
2. 整体的监控
3. 容错措施
4. 弹性伸缩
5. 分布式事务
6. 分布式锁

`往往解决了一个痛点又会引入别的痛点，所以架构的演进都是权衡的结果。`

`分布式事务是由多个本地事务组成的，`分布式事务跨越了多设备，之间又经历的复杂的网络。

### 2PC
> Two-phase commit protocal，两阶段提交协议。准备阶段和提交、回滚阶段
> 引入了一个事务协调者角色，来管理各个参与者（即各数据库资源）

![2pc](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/2pc.png)

2PC 事务协调者有超时机制

`2PC优点：`利用数据库自身功能进行本地事务的提交和回滚，即提交和回滚实际不需要我们实现，不侵入业务逻辑由数据库完成。

`2PC缺点：`同步阻塞、单点故障和数据不一致问题

1. 同步阻塞：第一阶段执行准备命令后。本地资源都处于锁定状态，效率低
2. 单点故障：即协调者单点，如果协调者挂了则整个事务就执行不下去了。
3. 数据不一致问题：由于网络的不稳定性，可能导致某些参与者无法收到协调者的请求，会导致各个参与者之间数据不一致

`小结：`
同步阻塞的强一致性两阶段提交协议，分为准备和提交/回滚阶段

2PC 优势在于对业务没有侵入



### 分布式数据库的 2PC 改进模型
> Percolator 模型，基于分布式存储系统 BigTable 建立的模型


### XA 规范
> XA 规范基于两阶段提交，实现了两阶段提交协议
> DTP （Distributed Transaction Processing）模型，规范了分布式事务的模型设计
> XA 规范约束了 DTP 模型中的事务管理器（TM）和资源管理器（RM）间的交互，即按照一定格式规范交互

AP -> TM
RM <-> TM
AP <-> RMv


* AP 应用程序：即应用，事务的发起者
* RM 资源管理器：可以简单任务是数据库，具备事务提交和回滚能力，对应2PC的参与者
* TM 事务管理器：协调者，和每个 RM 通信

### MySQL XA
> InnoDB 支持
> 简单来说就是要先定义一个全局唯一的 XID，然后告知每个事务分支要进行的操作。

![MySQL_XA](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/MySQL_XA.png)


### 3PC
> 解决2PC同步阻塞和减少数据不一致情况
>相比于2PC，多了一个询问阶段，即准备、预提交和提交

![3pc](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/3pc.png)

出发点好，但是事务绝大部分情况，因此每次都多了一个交互阶段
并引入超时机制，若协调者挂了，并已经到了提交阶段，参与者等待超时则自动提交事务

`2PC,3PC 小结：`

**2PC**
* 同步阻塞
* 单点故障
* 强一致性
* 数据不一致

**3PC**
* 性能差
* 想要解决2PC存在的问题，然后并没有

### TCC
> 2PC和3PC 都是依赖于数据库的事务提交和回滚
> TCC 是一个中业务层面或者应用层面的两阶段提交
> TCC 分为 Try、Confirm、Cancel，主要用于跨数据库、跨服务的业务操作的数据一致性问题
> 第一阶段是资源检查预留阶段，即Try。第二阶段是提交或者回滚

![TCC](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/TCC.png)

对业务有侵入，但TCC没有资源的阻塞

`TCC注意点：`
1. 幂等实现：因为网络调用无法保证请求稳定，因此有重试机制，因此对Try、Confirm、Cancel三个方法都需要幂等实现，避免重复执行
2. 空回滚：指Try方法由于网络问题没收且超时，此时事务管理器就会发出Cancel命令，即需要支持Cancel在未执行Try的情况下能正常的Canel
3. 防悬挂：指Try方法由于网络阻塞超时触发了事务管理器发出了Canel命令，但是执行了Cancel命令之后Try请求到了


### Seata
> Seata 是一款开源的分布式事务解决方案，致力于提高性能和简单易用的分布式事务服务，Seata为用户提供了AT、TCC、SAGA和XA事务模式，为用户打造一站式分布式解决方案


`AT模式`
通过回滚日志补偿恢复
需要引入全局锁的概念，否则会出现回滚了其他事务提交的数据

![seata-at](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/seata-at.png)

因为整个过程全局锁在 tx1 结束前一直是被 tx1 持有的，所以不会发生脏写的问题。

![seata-at-2](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/seata-at-2.png)

`TCC模式`
> 把自定义的分支事务纳入全局事务的管理中

![seata-tcc](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/seata-tcc.png)