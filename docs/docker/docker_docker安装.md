

# Docker安装

## ubuntu安装docker

本文档所用ubuntu版本为20.04.1 LTS, 安装过程参考自官方文档<https://docs.docker.com/engine/install/ubuntu/>

![1598578985174](docker安装.assets/1598578985174.png)



### 1. 更新`apt`软件包索引并安装软件包以允许`apt`通过HTTPS使用存储库

因为apt默认不支持https, 而docker的仓库是https的, 所以要先安装软件让apt支持https

```sh
sudo apt-get update
sudo apt-get install \
   apt-transport-https \
   ca-certificates \
   curl \
   gnupg-agent \
   software-properties-common
```

### 2. 添加 Docker 的官方 GPG 密钥

```sh
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

### 3. 设置稳定版仓库

1. 查看当前系统架构

   ```sh
   dpkg --print-architecture
   ```

2. 根据架构选用以下对应命令执行

   * amd64

     ```sh
     sudo add-apt-repository \
        "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
        $(lsb_release -cs) \
        stable"
     ```

   * armhf

     ```sh
     sudo add-apt-repository \
        "deb [arch=armhf] https://download.docker.com/linux/ubuntu \
        $(lsb_release -cs) \
        stable"
     ```

   * arm64

     ```sh
     sudo add-apt-repository \
        "deb [arch=arm64] https://download.docker.com/linux/ubuntu \
        $(lsb_release -cs) \
        stable"
     ```

### 4. 安装DOCKER引擎

* 更新`apt`程序包索引，并安装*最新版本*的Docker Engine

  ```sh
   sudo apt-get update
   sudo apt-get install docker-ce docker-ce-cli containerd.io
  ```

  

* 安装*特定版本*的Docker Engine

  1. 列出可用版本

     ```sh
     apt-cache madison docker-ce
     ```

     ![1598580699720](docker安装.assets/1598580699720.png)

  2. 安装指定版本,例如`5:19.03.12~3-0~ubuntu-focal`

     ```sh
     sudo apt-get install docker-ce=<版本号> docker-ce-cli=<版本号> containerd.io
     ```

     

### 5. 通过运行`hello-world` 映像来验证是否正确安装了Docker Engine

若镜像下载缓慢或下载失败，可参考附录配置阿里云仓库地址加速下载

```sh
sudo docker run hello-world
```

运行结果如下, 说明已正确安装
![1598581147741](docker安装.assets/1598581147741.png)





## CentOS安装docker

本文档所用CentOS版本为CentOS 7.0, 安装过程参考自官方文档<https://docs.docker.com/engine/install/centos/>

![1598586782936](docker安装.assets/1598586782936.png)

### 在线安装

#### 1. 卸载旧版本

较旧的Docker版本称为`docker`或`docker-engine`。如果已安装这些程序，请卸载它们以及相关的依赖项。

```sh
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

#### 2. 配置docker yum仓库

```sh
sudo yum install -y yum-utils

sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

#### 3. 安装DOCKER引擎

* 安装*最新版本*的Docker

  ```sh
  sudo yum install docker-ce docker-ce-cli containerd.io
  ```

  

* 安装*特定版本*的Docker

  1. 列出并排序您存储库中可用的版本

     ```sh
     yum list docker-ce --showduplicates | sort -r
     ```

     ![1599190901792](docker安装.assets/1599190901792.png)

  2. 安装指定版本

     版本号是第二列中从第一个冒号（`:`）到第一个连字符（`-`）之间的值，如上图所示

     ```sh
     sudo yum install docker-ce-<版本号> docker-ce-cli-<版本号> containerd.io
     ```

  PS: 此时Docker已安装但尚未启动。用户组`docker`已创建，但没有用户添加到该组。

#### 4. 启动docker

```sh
sudo systemctl start docker
```

#### 5. 通过运行`hello-world` 镜像来验证是否正确安装了Docker Engine 。

若镜像下载缓慢或下载失败，可参考附录配置阿里云仓库地址加速下载

```sh
sudo docker run hello-world
```



### 手动安装

#### 1. 卸载旧版本

较旧的Docker版本称为`docker`或`docker-engine`。如果已安装这些程序，请卸载它们以及相关的依赖项。

```sh
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

#### 2. 下载Docker安装包

下载地址: <https://download.docker.com/linux/centos/7/x86_64/stable/Packages/>

选择要安装的版本的`.rpm`文件下载

#### 3. 安装Docker

后面的文件路径换成实际的你下载的rpm包的路径

```sh
sudo yum install /path/to/package.rpm
```

PS: 此时Docker已安装但尚未启动。该`docker`组已创建，但没有用户添加到该组。

#### 4. 启动Docker

```sh
sudo systemctl start docker
```

#### 5. 通过运行`hello-world` 镜像来验证是否正确安装了Docker Engine

若镜像下载缓慢或下载失败，可参考附录配置阿里云仓库地址加速下载

```sh
sudo docker run hello-world
```



## windown7安装docker

本文档所用系统版本为 Window7 64位 旗舰版 , 安装过程参考自官方文档<https://docs.docker.com/toolbox/toolbox_install_windows/>

win7使用Docker Toolbox安装docker, 这是docker对旧版桌面系统安装docker的解决方案, 如果可以, 建议将系统升级到Windows 10 x64 pro（update 1607，Build 14393）或更之上, 以使用[Docker Desktop for Windows](https://docs.docker.com/docker-for-windows)

### 1. 检查系统版本

首先确保您的Windows系统支持硬件虚拟化技术，并且已启用虚拟化。其次确保确认您的Windows操作系统是64位（x64）。验证系统环境是否符合要求运行[Speccy](https://www.piriform.com/speccy)查看,  Speccy下载地址: https://www.piriform.com/speccy。

![1599025661419](docker安装.assets/1599025661419.png)

![1599025706088](docker安装.assets/1599025706088.png)

VMware虚拟机开启CPU虚拟化:

![1599016380805](docker安装.assets/1599016380805.png)

如果是虚拟机安装, 还需额外设置虚拟机网络连接

![1599029097723](docker安装.assets/1599029097723.png)

### 2. 安装docker

在本节中，您将安装Docker Toolbox软件和一些“帮助程序”应用程序。安装会将以下软件添加到您的计算机：

- Windows版Docker客户端
- Docker工具箱管理工具和ISO
- Oracle VM VirtualBox
- Git MSYS-git UNIX工具

安装步骤:

1. 要下载最新版本的Docker Toolbox，请到<https://github.com/docker/toolbox/releases>并下载最新`.exe`文件。

   ![1599013231437](docker安装.assets/1599013231437.png)

2. 通过双击安装程序来安装Docker Toolbox。

![1599026416402](docker安装.assets/1599026416402.png)

![1599026446942](docker安装.assets/1599026446942.png)

![1599026480519](docker安装.assets/1599026480519.png)

![1599026503224](docker安装.assets/1599026503224.png)

![1599026531475](docker安装.assets/1599026531475.png)

![1599026594719](docker安装.assets/1599026594719.png)

![1599029194778](docker安装.assets/1599029194778.png)

安装完成后桌面会出现如下三个图标:

![1599026665105](docker安装.assets/1599026665105.png)

### 3. 验证安装

右键“Docker Quickstart Terminal”选择以管理员身份运行，启动该程序后，会提示没有默认的“boot2docker”文件，开始从github上下载，**若网络好**则不会出现服务端将请求禁止等问题，会正确将boot2docker安装好，并顺利使用。如网络较差可自行下载并放到提示位置，具体下载链接和存放位置以实际提示为准。经测试，手动下载会快很多。

![1599026891300](docker安装.assets/1599026891300.png)

![1599014392739](docker安装.assets/2020-09-02_104846.png)

启动之后出现如下界面，说明docker已成功启动

![1599029710454](docker安装.assets/1599029710454.png)

最后输入`docker run hello-world`命令， 运行结果如下图所示则docker功能正常。

若镜像下载缓慢或下载失败，可参考附录配置阿里云仓库地址加速下载

![1599033448221](docker安装.assets/1599033448221.png)



## windows10安装docker

本文档所用系统版本为 Window10 专业版 64位 , 安装过程参考自官方文档<https://docs.docker.com/docker-for-windows/install/>

![image-20200903112044296](docker%E5%AE%89%E8%A3%85.assets/image-20200903112044296.png)

### 1. 系统要求

- Windows 10 64位：专业版，企业版或教育版（内部版本16299或更高版本）。

    对于Windows 10 Home，请参阅https://docs.docker.com/docker-for-windows/install-windows-home/。

- 必须启用Hyper-V和Containers Windows功能。

- 要在Windows 10上成功运行Client Hyper-V，需要满足以下硬件先决条件：

    - 具有[二级地址转换（SLAT）的](http://en.wikipedia.org/wiki/Second_Level_Address_Translation) 64位处理器
    - 4GB系统内存
    - 必须在BIOS设置中启用BIOS级硬件虚拟化支持。

#### 1.1. BIOS开启虚拟化支持

1. 重启电脑，使用快捷键进入电脑的bios设置（不同品牌的主板快捷键也不相同，可根据主板的品在百度上搜索，常用的有F2、Delete和Esc键）。

    ![img](docker%E5%AE%89%E8%A3%85.assets/ac6eddc451da81cb85aa53f99a09a01009243128.jpeg)

2. 在Bios内找到“Virtualization Technology”选项 (关键字是“VT”、“Virtual”或“Virtualization”, 一些Bios会是“VT-X”或“SVM”) ，汉化的Bios则是“Intel虚拟化技术” 。通常该选项会在bios的Advanced（高级）页面下的CPU选项内，如果没有的话还需要大家在Bios中耐心寻找。

    ![img](docker%E5%AE%89%E8%A3%85.assets/9f2f070828381f30e58f6e01616e3c0e6f06f07e.jpeg)

3. 将虚拟化技术设置成开启（Enabled）后，保存退出，cpu虚拟化就会保持在打开的状态了。

    ![img](docker%E5%AE%89%E8%A3%85.assets/8601a18b87d6277fa419a563e2576f36e824fcdc.jpeg)

#### 1.2.启用Hyper-V

1. 右键**开始**菜单， 并点击**应用和功能**

    ![image-20200903113220163](docker%E5%AE%89%E8%A3%85.assets/image-20200903113220163.png)

2. 拉到最底部点击**程序和功能**

    ![image-20200903113446258](docker%E5%AE%89%E8%A3%85.assets/image-20200903113446258.png)

3. 点击**启用或关闭Windows功能**

    ![image-20200903113514064](docker%E5%AE%89%E8%A3%85.assets/image-20200903113514064.png)

4. 勾选**Hyper-V**， 并点击**确定**按钮

    ![image-20200903113615089](docker%E5%AE%89%E8%A3%85.assets/image-20200903113615089.png)

5. **重启**计算机使更改生效

    ![image-20200903113958875](docker%E5%AE%89%E8%A3%85.assets/image-20200903113958875.png)

### 2. 下载

进入官网下载安装程序，官网：https://hub.docker.com/editions/community/docker-ce-desktop-windows/

![image-20200903114534996](docker%E5%AE%89%E8%A3%85.assets/image-20200903114534996.png)

![image-20200903114828710](docker%E5%AE%89%E8%A3%85.assets/image-20200903114828710.png)

### 3. 安装

1. 双击**Docker Desktop Installer.exe**运行安装程序。

    ![image-20200903115148090](docker%E5%AE%89%E8%A3%85.assets/image-20200903115148090.png)

2. 稍等片刻后就安装完成了，点击**Close and restart**重启计算机

    ![image-20200903115453295](docker%E5%AE%89%E8%A3%85.assets/image-20200903115453295.png)

### 4. 启动docker

1. 双击桌面的docker图标， 此时右下角将会出现鲸鱼动画图标

    ![image-20200903120239502](docker%E5%AE%89%E8%A3%85.assets/image-20200903120239502.png)![image-20200903120345513](docker%E5%AE%89%E8%A3%85.assets/image-20200903120345513.png)

2. 当状态栏中的鲸鱼图标保持稳定时，Docker桌面将启动并运行，并且可以从任何终端窗口访问。

    ![image-20200903133736208](docker%E5%AE%89%E8%A3%85.assets/image-20200903133736208.png)

    

### 5. 验证

输入`docker run hello-world`命令， 运行结果如下图所示则docker功能正常。

若镜像下载缓慢或下载失败，可参考附录配置阿里云仓库地址加速下载

​    ![1599190534002](docker安装.assets/1599190534002.png)

## MacOS安装docker

本文档所用系统版本为 MacOS 10.14 , 安装过程参考自官方文档<https://docs.docker.com/docker-for-mac/install/>

![1599190503333](docker安装.assets/1599190503333.png)

### 1. 系统要求

- **Mac硬件必须是2010或更新的型号**，并且Intel的硬件支持内存管理单元（MMU）虚拟化，包括扩展页表（EPT）和无限制模式。您可以通过在终端中运行以下命令来检查计算机是否具有此支持：`sysctl kern.hv_support`

  如果您的Mac支持Hypervisor框架，则命令显示`kern.hv_support: 1`。

- **macOS必须为10.13或更高版本**。

  如果将macOS升级到10.15版后遇到任何问题，则必须安装最新版本的Docker Desktop才能与此版本的macOS兼容。

  **注意：** Docker在最新版本的macOS上支持Docker Desktop。即，当前版本的macOS和前两个版本。Docker Desktop当前支持macOS Catalina，macOS Mojave和macOS High Sierra。

  随着新的主要版本的macOS普遍可用，Docker不再支持最旧的版本，而支持最新版本的macOS（除了前两个版本之外）。

- 至少4 GB的RAM。

- 不得安装4.3.30之前的VirtualBox，因为它与Docker Desktop不兼容。

### 2. 下载和安装

1. 下载安装包

    前往docker官网下载安装包， 官网地址：https://hub.docker.com/editions/community/docker-ce-desktop-mac/

    ![image-20200903174329922](docker%E5%AE%89%E8%A3%85.assets/image-20200903174329922.png)

2. 双击下载的安装程序Docker.dmg

    ![image-20200903174859386](docker%E5%AE%89%E8%A3%85.assets/image-20200903174859386.png)

3. 拖动Docker图标到Applications文件夹中

    ![image-20200903174948056](docker%E5%AE%89%E8%A3%85.assets/image-20200903174948056.png)

### 3. 启动docker

1. 在应用程序列表中双击Docker图标

    ![image-20200903175305914](docker%E5%AE%89%E8%A3%85.assets/image-20200903175305914.png)

2. 待顶部状态栏中的Docker图标从动画变成如下图所示的静止鲸鱼图案， 说明docker已启动成功

    ![image-20200903175635270](docker%E5%AE%89%E8%A3%85.assets/image-20200903175635270.png)

### 4. 验证

打开终端， 运行命令`docker run htllo-world`

若镜像下载缓慢或下载失败，可参考附录配置阿里云仓库地址加速下载

![image-20200903181250764](docker%E5%AE%89%E8%A3%85.assets/image-20200903181250764.png)

# 附录

## 非root用户管理docker

本文参考自官方文档：<https://docs.docker.com/engine/install/linux-postinstall/>

Docker守护程序绑定到Unix套接字而不是TCP端口。默认情况下，Unix套接字由`root`用户拥有，其他用户只能使用`sudo`来访问它。Docker守护程序始终以`root`用户身份运行。所以该文所介绍的方法是将docker所需的权限通过用户组传递到非root用户。据官方描述，完全以非root权限使用docker目前还是实验性的功能，有诸多限制，具体请参考官方文档：<https://docs.docker.com/engine/security/rootless/>

如果不想在运行`docker`命令时加`sudo`前缀，可以创建一个名为`docker`的用户组并将用户加入到该组。Docker守护程序启动时，它将创建一个可由该`docker`组成员访问的Unix套接字。

1. 创建`docker`用户组

   ```sh
   sudo groupadd docker
   ```

2. 将您的用户添加到该`docker`组($USER表示当前用户, 也可指定其他用户)

   ```sh
   sudo usermod -aG docker $USER
   ```

3. 激活对组的更改

   如果在虚拟机上进行测试，则可能需要重新启动虚拟机以使更改生效。

   ```
   newgrp docker 
   ```

4. 验证普通用户是否可以不带`sudo`前缀运行`docker`

   ```sh
   docker run hello-world
   ```

   如果在将用户添加到组之前就运行过Docker CLI命令, 就可能会出现以下错误

   ![1598584152815](docker安装.assets/1598584152815.png)

   *  解决方法一:

     删除`~/.docker/`目录（会自动重新创建目录，但是所有自定义设置都会丢失）

   * 解决方法二:

     运行以下命令

     ```sh
     sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
     sudo chmod g+rwx "$HOME/.docker" -R
     ```



## 配置阿里云仓库地址

因为docker的中央仓库在国外， 所以下载镜像时经常因为网络原因导致下载速度缓慢甚至下载失败。因此我们要通过配置国内仓库地址来加快下载速度。

### 1. 获取阿里云仓库地址

1. 进入阿里云官网点击登录

    官网地址：https://www.aliyun.com

    ![image-20200903135002722](docker%E5%AE%89%E8%A3%85.assets/image-20200903135002722.png)

2. 使用支付宝扫码登陆

    ![image-20200903135110410](docker%E5%AE%89%E8%A3%85.assets/image-20200903135110410.png)

3. 登录完成后点击控制台

    ![image-20200903135217734](docker%E5%AE%89%E8%A3%85.assets/image-20200903135217734.png)

4. 进入控制台后展开菜单并点击容器镜像服务

    ![image-20200903135437892](docker%E5%AE%89%E8%A3%85.assets/image-20200903135437892.png)

5. 点击镜像加速器，下图箭头所指位置便是阿里云仓库地址

    ![image-20200903135652356](docker%E5%AE%89%E8%A3%85.assets/image-20200903135652356.png)

### 2. 配置阿里云仓库地址

#### ubuntu配置

1. 运行如下命令

   ```sh
   sudo mkdir -p /etc/docker
   # 这里registry-mirrors 后面要替换为你想要的替换的镜像源
   sudo tee /etc/docker/daemon.json <<-'EOF'
   {
     "registry-mirrors": ["https://xxxxxxxx.mirror.aliyuncs.com"]
   }
   EOF
   sudo systemctl daemon-reload
   sudo systemctl restart docker
   ```

   ![1599185838993](docker安装.assets/1599185838993.png)

2. 运行`docker info`命令查看配置是否生效

   ![1599185934382](docker安装.assets/1599185934382.png)

3. 配置完成后可以测试下载镜像，速度非常快

   ![1599186322410](docker安装.assets/1599186322410.png)

#### CentOS配置

1. 运行如下命令

   ```sh
   sudo mkdir -p /etc/docker
   # 这里registry-mirrors 后面要替换为你想要的替换的镜像源
   sudo tee /etc/docker/daemon.json <<-'EOF'
   {
     "registry-mirrors": ["https://xxxxxxxx.mirror.aliyuncs.com"]
   }
   EOF
   sudo systemctl daemon-reload
   sudo systemctl restart docker
   ```

   ![1599189026086](docker安装.assets/1599189026086.png)

2. 运行`docker info`命令查看配置是否生效

   ![1599189089991](docker安装.assets/1599189089991.png)

3. 配置完成后可以测试下载镜像，速度非常快

   ![1599189153844](docker安装.assets/1599189153844.png)

#### windows7配置

1. 运行命令start $DOCKER_CERT_PATH，将会打开一个文件夹

   ![1599183174651](docker安装.assets/1599183174651.png)

   ![1599183260415](docker安装.assets/1599183260415.png)

2. 编辑打开的文件夹中的config.json文件，将仓库地址填入下图所示位置，修改好配置文件后保存。

   ![1599183421832](docker安装.assets/1599183421832.png)

3. 运行以下命令

   ```sh
   docker-machine ssh default
   
   # 这里--registry-mirror 后面要替换为你想要的替换的镜像源
   sed -e \
   "/--label provider/a\--registry-mirror https://xxxxxxxx.mirror.aliyuncs.com" \
   /var/lib/boot2docker/profile
   
   sudo /etc/init.d/docker restart
   
   exit
   
   # 重启 docker-machine
   docker-machine restart default
   ```

   ![1599184143854](docker安装.assets/1599184143854.png)

4. 重新启动Docker Quickstart Terminal， 并运行doker info命令查看更改是否生效

   ![1599184620450](docker安装.assets/1599184620450.png)

   ![1599184574714](docker安装.assets/1599184574714.png)

5. 配置完成后可以测试下载镜像，速度非常快

   ![1599184730296](docker安装.assets/1599184730296.png)

#### windows10配置

1. 右键鲸鱼小图标，并点击settings

    ![image-20200903140521135](docker%E5%AE%89%E8%A3%85.assets/image-20200903140521135.png)

2. 在弹出的窗口中点击Docker Engine，按如下图所示将配置好仓库地址，配置好后点击右下方Apply&Restart按钮重启docker

    ![image-20200903140847470](docker%E5%AE%89%E8%A3%85.assets/image-20200903140847470.png)

3. docker重启完成之后可通过`docker info`命令查看配置是否生效

    ![image-20200903141305257](docker%E5%AE%89%E8%A3%85.assets/image-20200903141305257.png)

4. 配置完成后可以测试下载镜像，速度非常快

    ![image-20200903141622632](docker%E5%AE%89%E8%A3%85.assets/image-20200903141622632.png)

    

#### MacOS配置

1. 点击顶部状态栏的鲸鱼图标， 然后点击Preferences...

    ![image-20200903180352647](docker%E5%AE%89%E8%A3%85.assets/image-20200903180352647.png)

2. 在弹出的窗口中点击Docker Engine，按如下图所示将配置好仓库地址，配置好后点击右下方Apply&Restart按钮重启docker

    ![image-20200903180652379](docker%E5%AE%89%E8%A3%85.assets/image-20200903180652379.png)

3. docker重启完成之后可通过`docker info`命令查看配置是否生效

    ![image-20200903180936047](docker%E5%AE%89%E8%A3%85.assets/image-20200903180936047.png)

4. 配置完成后可以测试下载镜像，速度非常快

    ![image-20200903181058587](docker%E5%AE%89%E8%A3%85.assets/image-20200903181058587.png)

