# kubernetes

- [kubernetes](#kubernetes)
  - [JD](#jd)
    - [云原生架构师 30-45K·13薪](#云原生架构师-30-45k13薪)
  - [一、概念](#一概念)
    - [1.1. 概述](#11-概述)
    - [1.1.1. 时光回溯](#111-时光回溯)
    - [1.1.2. kubernetes 能做什么：](#112-kubernetes-能做什么)
    - [1.1.3. kubernetes 不是什么](#113-kubernetes-不是什么)
    - [1.1.4. kubernetes 组件](#114-kubernetes-组件)
    - [1.1.5. Node 组件](#115-node-组件)
    - [1.1.6. 插件（Addons）](#116-插件addons)
    - [1.1.6. kubernetes API](#116-kubernetes-api)
    - [1.1.7. kubernetes 对象](#117-kubernetes-对象)

> kubernetes 是一个开源的容器编排引擎，用来对容器化应用进行自动化部署、扩缩和管理。

## JD
### 云原生架构师 30-45K·13薪
【岗位要求】

```jd
1、本科及以上学历，在产品设计领域、技术架构领域或产品研发管理领域有五年及以上工作经验，有跨两个及以上领域经验者优先
2、熟悉K8S、容器、Serverless等容器和云原生技术，了解微服务架构、分布式架构、敏捷开发、DevOps流程，有相关技术落地经验者优先。
3、有较强的技术背景能力，熟悉云计算、负载均衡、数据库（常用的关系型数据库、NoSQL数据库）、分布式缓存、消息服务、中间件等技术，参与大型项目开发或架构设计者优先
4、熟悉产品设计、开发工作流程，能洞察客户需求产出MRD、DEMO、PRD，有电商领域、金融领域、物联网、人工智能领域产品方案设计与落地经验者优先
5、熟悉阿里云大数据、安全产品、数据库、中间件产品，或者有相关基于阿里云产品应用设计或开发经验者优先
6、拥有PMP证书、NPDP证书、Scrum Master、TOGAF认证者优先
7、良好的沟通协调能力，强烈的责任心，性格稳重、踏实、细心、思考问题思路清晰
```

## 一、概念
> 了解 kubernetes 的各个组成部分

### 1.1. 概述

**kubernetes 是什么？**
> kubernetes 是一个可移植、可扩展的开源平台，用于管理容器化的工作负载和服务，可促进声明式配置和自动化。kubernetes 拥有一个庞大且快速增长的生态系统。kubernetes 的服务、支持和工具广泛可用

> kubernetes 源于希腊语，意为"舵手"或"飞行员"
> Google 在 2014 年开源 kubernetes 项目
> Google 在大规模运行生产工作负载方面有十几年的经验


### 1.1.1. 时光回溯
![](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/container_evolution.svg)

**传统部署时代：**
早期，在物理服务器上运行应用程序。无法为应用程序定义资源边界，导致资源分配问题。可能出现某个应用程序占用大部分资源，同时导致其他应用程序的性能下降。

**虚拟化部署时代：**
引入虚拟化，虚拟化技术允许在单个物理服务器上运行多个虚拟机。虚拟化运行应用程序在虚拟机之间隔离。虚拟化技术能够更好地利用物理服务器上的资源。

**容器部署时代：**
容器类似于VM，具有被放宽的隔离属性，开源在应用程序之间共享OS，具有自己的文件系统、CPU、内存、进程空间等

`容器优点`

1. 敏捷应用程序的创建和部署：与VM相比，提高了容器镜像创建的简便性和效率
2. 持续开发、集成和部署：通过快速简单的回滚，支持可靠且频繁的容器镜像构建和部署
3. 关注开发与运维分离：在构建/发布时而不是在部署时创建应用程序容器镜像，从而将应用程序与基础架构分离
4. 可观察性：显示OS级别的信息和指标，还可以显示应用程序的运行状况和其他指标信号
5. 跨开发、测试和生产的环境一致性：在便携式计算机上与在云中相同运行
6. 跨云和OS发行版本的可以执行：可在 Ubuntu、CoreOS、本地、Google Kubernetes Engine 和其他任何地方运行
7. 以应用程序为中心和管理：提高抽象级别，从虚拟硬件上运行OS到使用逻辑资源在OS上运行应用程序
8. 松散耦合、分布式、弹性、解放的微服务：应用程序被分解为较小的独立部分，并且可以动态部署和管理-而不是在一台大型单机上整体运行
9. 资源隔离：可预测的应用程序性能
10. 资源利用：高效率和高密度

### 1.1.2. kubernetes 能做什么：
> kubernetes 提供了一个可弹性运行分布式系统的框架。kubernetes 会满足我们扩展要求、故障转移、部署模式等

* 服务发现和负载均衡：kubernetes 使用 DNS 名称或自己的 IP 地址公开容器，如果进入容器的流量很大，kubernetes 可以负载均衡并分配网络流量，从而使部署稳定
* 存储编排：kubernetes 运行我们自动挂载选择的存储系统，如本地存储、公共云提供商等
* 自动部署和回滚：可以使用kubernetes描述已部署容器的所需状态，以受控的速率将实际状态更改为期望状态。如可以自动化 kubernetes 为我们部署创建新容器，删除现有容器并将它们的所有资源用于新容器
* 自动完成装箱计算：运行指定每个容器所需的 CPU 和内存（RAM）。当容器制定了资源请求时，kubernetes 可以做出更好地决策来管理容器的资源
* 自我修复：kubernetes 重新启动失败的容器、替换容器、杀死不响应用户定义的运行状况检查的容器，并且在准备好服务之间部将其通告给客户端
* 密钥与配置管理：运行我们存储和管理敏感信息，例如密码、OAuth 令牌和 ssh 密钥。可以在不重建容器镜像的情况下部署和更新密钥和应用程序配置，也无需在对战配置中暴露密钥

### 1.1.3. kubernetes 不是什么
> 不是单体系统，默认解决方案都是可选和可插拔的。
> 由于 kubernetes 在容器级别而不是在硬件运行，提供了 PaaS 产品共有的一些普遍适用的功能，如部署、扩展、负载均衡、日志记录和监视。

* 不限制支持的应用程序类型。kubernetes 旨在支持极多种多样的工作负载，包括无状态、有状态和数据处理工作负载。
* 不部署源代码，也不构建应用程序。持续集成（CI）、交付和部署（CI、CD）工作流取决于组织的文化和偏好以及技术要求
* 不提供应用程序级别的服务作为内置服务，如中间件（如 消息中间件）、数据处理框架（如 Spark）、数据库（如 MySQL）、缓存、集群存储系统（如 Ceph）
* 不要求日志记录、监控或警报解决方案。提供了一些集成作为概念证明，并提供了收集和导出指标的机制
* 不提供也不采用任何全面的机器配置、维护、管理或自我修复系统

### 1.1.4. kubernetes 组件
> kubernetes 集群由一组被称为节点的机器组成。集群至少一个工作节点。
> 工作节点托管作为应用负载的组件 Pod。控制平面管理集群中的工作节点和 Pod。为集群提供故障转移和高可用性

![所有相互关联组件](https://cdn.jsdelivr.net/gh/wudg/picgo@master/images/blog/components-of-kubernetes.svg)

**控制平面组件**
> 控制平板的组件对集群做出全局决策（如调度），以及检测和响应集群事件（如 当不满足部署的 replicas 字段时，启动新的 Pod）
> 控制平面组件可以在集群中任何节点上运行。

**kube-apiserver**
> API 服务器时 kubernetes 控制面的组件，该组件公开了 kubernetes API。API 服务器时 kubernetes 控制面的前端
> kubernetes API 服务器的主要实现是 kube-apiserver。kube-apiserver 设计上考虑了水平伸缩，即可通过部署多个实例进行伸缩。

**etcd**
> etcd 是兼具一致性和高可用性的键值数据库，可以作为保存 kubernetes 所有集群数据的后台数据库。

**kube-scheduler**
> 控制平面组件，负责监视新创建的、未指定运行节点（node）的Pods，选择节点让1 Pod 在上面运行
> 调度决策考虑的因素包括单个 Pod 和 Pod 集合的资源需求、硬件/软件/策略约束、亲和性和反亲和性规范、数据位置、工作负载间的干扰和最后时限。

**kube-controller-manager**
> 运行控制器进程的控制平面组件

控制器：
1. 节点控制器（Node Controller）：负责在节点出现故障时进行通知和响应
2. 任务控制器（Job Controller）：检测代表一次性任务的 Job 对象，然后创建 Pods 来运行这些任务直至完成
3. 端点控制器（Endpoints Controller）：填充端点（Endpoints）对象（即加入 Service 与 Pod）
4. 服务账户和令牌控制器（Service Account & Token Controllers）：为新的命名控件创建默认账户和 API 访问令牌

**cloud-controller-manager**
> 云控制器管理器是指嵌入特定云的控制逻辑的控制平面组件。云控制器管理器使得我们可以将集群连接到云提供商的 API 之上，并将与该云平台交互的组件同与集群交互的组件分离开来。

### 1.1.5. Node 组件
> 节点组件在每个节点上运行，维护运行的 Pod 并提供 kubernetes 与运行环境

**kubelet**
> 一个在集群中每个节点（node）上运行的代理。它保证容器（containers）都运行在 Pod 中
> kubelet 接收一组通过各类机制提供给它们的 PodSpecs，确保这些 PodSpecs 中描述的容器处于运行状态且健康。kubelet 不会管理不是由 kubernetes 创建的容器

**kube-proxy**
> kube-proxy 是集群中每个节点上运行的网络代理，实现 kubernetes 服务（Service）概念的一部分
> kube-proxy 维护节点上的网络规则。这些网络规则运行从集群内部或外部的网络会话与 Pod 进行网络通信

**容器运行时（Container Runtime）**
> 容器运行环境是负责运行容器的软件
> kubernetes 支持多个容器运行环境：Docker、containerd、CRI-O 以及任何实现 kubernetes CRI（容器运行胡那就接口）

### 1.1.6. 插件（Addons）
> 插件适用 kubernetes 资源（DaemonSet、Deployment等）实现集群功能。插件提供集群级别的功能，插件中命名空间资源属于 kube-system 命名空间

**DNS**
> 几乎所有 kubernetes 集群都应该由 集群 DNS
> 集群 DNS 是一个 DNS 服务器，和环境中的其他 DNS 服务器一起工作，它为 kubernetes 服务提供 DNS 记录
> kubernetes 启动的容器自动将此 DNS 服务器包含在其 DNS 搜索列表中

**Web 界面（仪表盘）**
> Dashboard 是 kubernetes 集群通用的、基于 Web 的用于界面。它使用户可以管理集群中运行的应用程序以及集群本身并进行故障排除

**容器资源监控**
> 容器资源监控将关于容器的一些常见的时间序列度量值保存到一个集中的数据库中，并提供用于浏览这些数据的界面

**集群层面日志**
> 集群层面日志机制负责将容器的日志数据保存到一个集中的日志存储中，该存储能够提供搜索和浏览接口

### 1.1.6. kubernetes API
> kubernetes 控制面的核心是 API 服务器。API 服务器负责提供 HTTP API，以供用户、集群中的不同部分和集群外部组件相互通信
> kubernetes API 使我们可以查询和操纵 kubernetes API 中对象（如 Pod、Namespace、ConfigMap 和 Event）的状态
> 大部分操作都可以通过 kubectl 命令行接口或类似 kubeadm 这类命令行工具来执行，这些工具在背后也是调用 API

**OpenAPI 规范**
> kubernetes API 服务器通过 /openapi/v2 末端提供 OpenAPI 规范
> kubernetes 为 API 实现了一种基于 Protobuf 的序列化格式，主要用于集群内部通信

https://kubernetes.io/zh/docs/concepts/overview/kubernetes-api/

### 1.1.7. kubernetes 对象
> 说明 kubernetes 对象在 kubernetes API 中如何表示，以及如何在 .yaml 格式的文件中表示

**理解 kubernetes 对象**
> 在 kubernetes 系统中， kubernetes 对象是持久化的实体。kubernetes 适用这些实体去表示整个集群的状态。描述以下信息：
> * 那些容器应用在运行
> 可以被应用使用的资源
> 关于应用运行时表现的策略，比如重启策略、升级策略，以及容错策略

kubernetes 对象是"目标性记录",--一旦创建对象，kubernetes 系统将持续工作以确保对象存在。通过创建对象，本质上是告知kubernetes系统，所需要的集群工作负载看起来是什么样子的，这就是kubernetes集群的 `期待状态`（Desired State）
操作kubernetes对象--无论是创建、修改、或者删除--需要使用kubernetes API。如，当使用 `kubectl` 命令行接口时，CLI 会执行必要的kubernetes API调用，也可以在程序中使用客户端库直接调用 kubernetes API

**对象规约（Spec）与状态（Status）**
> 每个 kubernetes 对象包含两个嵌套的对象字段，它们负责管理对象的配置：对象 spec（规约）和对象 statue（状态）。对于具有 spec 的对象，必须在创建对象时设置其内容，描述我们希望对象所具有的特征：期望状态

**描述kubernetes对象**
> 创建kubernetes对象时，必须提供对象的规约，用于描述该对象的期待状态，以及关于对象的一些基本信息（如名称）。当使用kubernetes API创建对象时（或者直接创建，或者基于kubectl），API请求必须在请求体中包含JSON格式的信息。大多数情况下，需要在.yaml文件中为 kubectl 体哦国内这些信息。kubectl 在发起API请求时，将这些信息转换成JSON格式

下面 .yaml 示例，展示了 kubernetes Deployment 必须字段和对象规约：

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 # tells deployment to run 2 pods matching the template
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx

        image: nginx:1.14.2
        ports:
        - containerPort: 80

```
使用类似于上面的 .yaml 文件来创建 Deployment 的一种方式是使用 kubectl 命令行接口（CLI）中的 `kubectl apply`命令,将 .yaml 文件作为参数，下面是一个示例：

```shell
kubectl apply -f https://k8s.io/examples/application/deployment.yaml --record
```

输出：
```shell
deployment.apps/nginx-deployment created
```
**必须字段**
* apiVersion：创建该对象所使用 kubernetes API版本
* kind：对象类别
* metadata：帮助唯一性标识对象的一些数据，包括一个 name 字符串、UID和可选的 namespace
* spec：期望该对象的状态
