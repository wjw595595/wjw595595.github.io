





# docker实战教程

## docker介绍

https://docs.docker.com/develop/

https://docs.docker.com/ 官方文档 	

https://docs.docker.com/compose/compose-file/compose-file-v3/

https://docs.docker.com/engine/reference/commandline/cli/

https://docs.docker.com/compose/install/

好参考：

https://www.wenjiangs.com/doc/3sfo9ygs8

**Docker 属于 Linux 容器的一种封装，提供简单易用的容器使用接口。**它是目前最流行的 Linux 容器解决方案。

Docker 将应用程序与该程序的依赖，打包在一个文件里面。运行这个文件，就会生成一个虚拟容器。程序在这个虚拟容器里运行，就好像在真实的物理机上运行一样。有了 Docker，就不用担心环境问题。容器是在操作系统层面上实现虚拟化，直接复用本地主机的操作系统

![img](https://i.loli.net/2020/10/15/KYPRw7EloN2QZG1.png)

### docker用途

**（1）提供一次性的环境。**比如，本地测试他人的软件、持续集成的时候提供单元测试和构建的环境。

**（2）提供弹性的云服务。**因为 Docker 容器可以随开随关，很适合动态扩容和缩容。

**（3）组建微服务架构。**通过多个容器，一台机器可以跑多个服务，因此在本机就可以模拟出微服务架构。

### docker优点

1、解决环境配置难题

2、虚拟机也可以解决环境配置问题,存在的问题1、占用的资源多（系统级）：占用磁盘和内存资源不共享，docker是进程级（资源可以共享）；冗余步骤多（比如登录）；启动慢

**Linux 容器不是模拟一个完整的操作系统，而是对进程进行隔离。**容器里面的进程来说，它接触到的各种资源都是虚拟的，从而实现与底层系统的隔离

### docker版本变化

docker版本变化：

Docker从1.13.x版本开始，版本分为企业版EE和社区版CE，版本号也改为按照时间线来发布，比如17.03就是2017年3月。

Docker的linux发行版的软件仓库从以前的https://apt.dockerproject.org和https://yum.dockerproject.org变更为目前的https://download.docker.com, 软件包名字改为docker-ce和docker-ee。

## docker安装

### 安装前准备

关闭 安全模块 selinux，不然docker会自动关闭

临时关闭： setenfore 0

永久关闭（需要重启）：修改/etc/selinux/config 文件

将SELINUX=enforcing改为SELINUX=disabled

### docker在线安装

centos7版本

1.查看Linux核心版本，3.10版本及以上才可以安装docker。

uname -r

2.更新yum包

yum update

3.查看docker是否曾经安装过

whereis docker

如果安装过，则删除之前的版本

yum remove docker docker-common docker-selinux docker-engine

4.安装需要的软件包， yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的

yum install -y yum-utils device-mapper-persistent-data lvm2

5.设置yum源

yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

6.查看docker版本，一般使用稳定版

yum list docker-ce --showduplicates | sort -r

![image-20200924092722161](https://i.loli.net/2020/09/24/O52JKtgZphXr7ex.png)

7.安装docker

默认安装最新版本

yum install docker-ce

安装某特定版本需增加版本号（如18.06.3.ce-3.el7）

yum install docker-ce-18.06.3.ce

8.启动docker

systemctl start docker

9.开机启动

systemctl enable docker

10. 验证安装是否成功

docker version

或

docker info

如下存在Client和Server则成功

![image-20200924093044991](https://i.loli.net/2020/09/24/CrG7nydLRPTVBal.png)

### docker离线安装

1、下载: https://download.docker.com/linux/static/stable/x86_64/

2、解压： tar -xvf docker-18.06.1-ce.tgz

3、将解压出来的docker文件内容移动到 /usr/bin/ 目录下
cp docker/* /usr/bin/
4、将docker注册为service
vim /etc/systemd/system/docker.service

`[Unit]
Description=Docker Application Container Engine
Documentation=https://docs.docker.com
After=network-online.target firewalld.service
Wants=network-online.target

[Service]
Type=notify
//the default is not to use systemd for cgroups because the delegate issues still
// exists and systemd currently does not support the cgroup feature set required
// for containers run by docker
ExecStart=/usr/bin/dockerd
ExecReload=/bin/kill -s HUP $MAINPID
// Having non-zero Limit*s causes performance problems due to accounting overhead
// in the kernel. We recommend using cgroups to do container-local accounting.
LimitNOFILE=infinity
LimitNPROC=infinity
LimitCORE=infinity
//Uncomment TasksMax if your systemd version supports it.
//Only systemd 226 and above support this version.
//TasksMax=infinity
TimeoutStartSec=0
//set delegate yes so that systemd does not reset the cgroups of docker containers
Delegate=yes
// kill only the docker process, not all processes in the cgroup
KillMode=process
//restart the docker process if it exits prematurely
Restart=on-failure
StartLimitBurst=3
StartLimitInterval=60s

[Install]
WantedBy=multi-user.target

`

4、启动

chmod +x /etc/systemd/system/docker.service             #添加文件权限并启动docker

systemctl daemon-reload                                                       #重载unit配置文件

systemctl start docker                                                             #启动Docker

systemctl enable docker.service                                           #设置开机自启

5、验证

systemctl status docker                                                         #查看Docker状态

docker -v                                                                                     #查看Docker版本

### docker-compose

#### docker-compose介绍

参考文档：https://docs.docker.com/compose/

​		Docker Compose是一个用来定义和运行多个Docker应用的工具。一个使用Docker容器的应用，通常由多个容器组成。使用Docker Compose不再需要使用shell脚本来启动容器。 
​		Compose 通过一个配置文件来管理多个Docker容器，在配置文件中，所有的容器通过services来定义，然后使用docker-compose脚本来启动，停止和重启应用，和应用中的服务以及所有依赖服务的容器，非常适合组合使用多个容器进行开发的场景。  

​    和docker版本的兼容

![image-20200924095350098](https://i.loli.net/2020/09/24/d7qbVwghGaCjMJy.png)





#### docker-compose安装

https://docs.docker.com/compose/install/

##### 从github安装

下载最新的 docker-compose文件

https://github.com/docker/compose/releases/tag/1.25.0  ：到github直接下载

`sudo curl -L https://github.com/docker/compose/releases/download/1.16.1/docker-compose-uname -s-uname -m -o /usr/local/bin/docker-compose`

 若是github访问太慢，可以用daocloud下载

`sudo curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.1/docker-compose-uname -s-uname -m -o /usr/local/bin/docker-compose`

添加可执行权限

sudo chmod +x /usr/local/bin/docker-compose

测试安装结果

$ docker-compose version

##### 通过python pip 安装

1、安装python-pip

yum -y install epel-release

yum -y install python-pip

2、安装docker-compose

pip install docker-compose

待安装完成后，执行查询版本的命令，即可安装docker-compose

docker-compose version



## docker核心原理

overlay2：存储驱动

## docker网络

Docker使用Linux桥接，在宿主机虚拟一个Docker容器网桥(docker0)，Docker启动一个容器时会根据Docker网桥的网段分配给容器一个IP地址，称为Container-IP，同时Docker网桥是每个容器的默认网关。因为在同一宿主机内的容器都接入同一个网桥，这样容器之间就能够通过容器的Container-IP直接通信。

**四种网络模式**

| Docker网络模式 | 配置                       | 说明                                                         |
| -------------- | -------------------------- | ------------------------------------------------------------ |
| host模式       | --net=host                 | 容器和宿主机共享Network namespace。                          |
| container模式  | --net=container:NAME_or_ID | 容器和另外一个容器共享Network namespace。 kubernetes中的pod就是多个容器共享一个Network namespace。 |
| none模式       | --net=none                 | 容器有独立的Network namespace，但并没有对其进行任何网络设置，如分配veth pair 和网桥连接，配置IP等。 |
| bridge模式     | --net=bridge               | （默认为该模式）                                             |



docker network ls #查看网络命令

当创建docker 时自动创建三个网络

![image-20201013133738430](https://i.loli.net/2020/10/13/SZM8mFH3r1d45ah.png)

### bridge模式

当Docker进程启动时，会在主机上创建一个名为docker0的虚拟网桥，此主机上启动的Docker容器会连接到这个虚拟网桥上。虚拟网桥的工作方式和物理交换机类似，这样主机上的所有容器就通过交换机连在了一个二层网络中。

从docker0子网中分配一个IP给容器使用，并设置docker0的IP地址为容器的默认网关。在主机上创建一对虚拟网卡veth pair设备，Docker将veth pair设备的一端放在新创建的容器中，并命名为eth0（容器的网卡），另一端放在主机中，以vethxxx这样类似的名字命名，并将这个网络设备加入到docker0网桥中。可以通过brctl show命令查看。

使用docker run -p时，docker实际是在iptables做了DNAT规则，实现端口转发功能。可以使用iptables -t nat -vnL查看

![image-20201013143115102](https://i.loli.net/2020/10/13/2AtEBTK16QpMjUH.png)

### host模式

**相当于Vmware中的桥接模式，与宿主机在同一个网络中，但没有独立IP地址**

docker 使用linux的namespaces技术来进行资源隔离，一个Network Namespace提供了一份独立的网络环境，包括网卡、路由、Iptable规则等都与其他的Network Namespace隔离。一个Docker容器一般会分配一个独立的Network Namespace。但如果启动容器的时候使用host模式，那么这个容器将不会获得一个独立的Network Namespace，而是和宿主机共用一个Network Namespace。容器将不会虚拟出自己的网卡，配置自己的IP等，而是使用宿主机的IP和端口不需要进行NAT，host最大的优势就是网络性能比较好，但是docker host上已经使用的端口就不能再用了，网络的隔离性不好。

![image-20201013140517603](https://i.loli.net/2020/10/13/UxahPb6G1YCFHJe.png)

### container模式

这个模式指定新创建的容器和已经存在的一个容器共享一个 Network Namespace，而不是和宿主机共享。新创建的容器不会创建自己的网卡，配置自己的 IP，而是和一个指定的容器共享 IP、端口范围等。同样，两个容器除了网络方面，其他的如文件系统、进程列表等还是隔离的。两个容器的进程可以通过 lo 网卡设备通信。

![image-20201013145550586](https://i.loli.net/2020/10/13/bsk1PnpjMmxECiO.png)

### None 网络模式

使用none模式，Docker容器拥有自己的Network Namespace，但是，并不为Docker容器进行任何网络配置。也就是说，这个Docker容器没有网卡、IP、路由等信息。需要我们自己为Docker容器添加网卡、配置IP等。

这种网络模式下容器只有lo回环网络，没有其他网卡。none模式可以在容器创建时通过--network=none来指定。这种类型的网络没有办法联网，封闭的网络能很好的保证容器的安全性

![image-20201013145956643](https://i.loli.net/2020/10/13/XTdqzVk2InZ9Rcl.png)

## volume（卷）

### Docker的数据持久化主要有两种方式

持久化使数据不随着container的结束而结束

- bind mount（host的某个指定目录中）

- volume（/var/lib/docker/volumes  docker自己管理volume）

#### bind mount

缺点：不同宿主机不好移植（系统目录结构不一样，不在dockerfile出现）

例子：

```shell
docker run -it -v $(pwd)/host-dava:/container-data alpine sh
```

**注意**

- host机器的目录路径必须为全路径(准确的说需要以`/`或`~/`开始的路径)，不然docker会将其当做volume而不是bind mount处理

- 如果host机器上的目录不存在，docker会自动创建该目录

- 如果container中的目录不存在，docker会自动创建该目录

- 如果container中的目录已经有内容，那么docker会使用host上的目录将其覆盖掉

#### volume

docker管理，在目录 /var/lib/docker/volumes

将my-volume挂载到container中的/mydata目录

```shell
docker run -it -v my-volume:/mydata alpine sh
#my-volume不存在，那么docker会自动创建my-volume 在/var/lib/docker/volumes 下
```

```shell
#查看
docker volume inspect my-volume
[
    {
        "CreatedAt": "2018-03-28T14:52:49Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/my-volume/_data",
        "Name": "my-volume",
        "Options": {},
        "Scope": "local"
    }
]
```

也可以不指定host名称

```shell
docker run -it -v /mydata alpine sh
```

- 如果volume是空的而container中的目录有内容，那么docker会将container目录中的内容拷贝到volume中

- 如果volume中已经有内容，则会将container中的目录覆盖

#### Dockerfile中

```shell
#Dockerfile
VOLUME /foo
```

  在docker运行时，docker会创建一个匿名的volume，并将此volume绑定到container的/foo目录中，如果container的/foo目录下已经有内容，则会将内容拷贝的volume中。也即，Dockerfile中的`VOLUME /foo`与`docker run -v /foo alpine`的效果一样

Dockerfile中的VOLUME使每次运行一个新的container时，都会为其自动创建一个匿名的volume，如果需要在不同container之间共享数据，那么我们依然需要通过`docker run -it -v my-volume:/foo`的方式将/foo中数据存放于指定的my-volume中

### 匿名卷

/var/lib/docker/volumes/区域

主机上的一个文件映射到容器当中的某个文件，容器向其中写入数据就相当于向主机中对应的文件写入数据

### volumes-from

1. 创建新容器时, 使用和另一个容器相同的挂载策略

2. docker container run --name d2 --volumes-from d1 -d nginx

   d1有自己的卷，d2集成d1的卷

### 命令

docker volume -help

```shell
#查看
$docker volume ls 
#查看详情
$docker volume inspect  <volumeName>
#创建
$docker volume crreate <volumeName>
# 删除卷
$docker volume rm <volumeName> 
# 删除没有挂载的卷,闲置的
$docker volume prune
```



## docker常用命令

![image-20210309200756693](https://i.loli.net/2021/03/09/lt8CbO9pJZWnE27.png)

![image-20210309200836981](https://i.loli.net/2021/03/09/vGm9f3WR6BO5MlI.png)

![image-20210309200912266](https://i.loli.net/2021/03/09/cHPeRs3JpI2Giv9.png)

![image-20210309200950938](https://i.loli.net/2021/03/09/AvGidrqXtUcPpVC.png)

![image-20210309201038189](https://i.loli.net/2021/03/09/4OjiwYqu352MFhC.png)

![image-20210309201109002](https://i.loli.net/2021/03/09/RkSTCjsUyIiBGqN.png)

## docker模块

### images文件和container文件

**Docker 把应用程序及其依赖，打包在 image 文件里面。**只有通过这个文件，才能生成 Docker 容器。image 文件可以看作是容器的模板。Docker 根据 image 文件生成容器的实例。同一个 image 文件，可以生成多个同时运行的容器实例。image 是二进制文件。实际开发中，一个 image 文件往往通过继承另一个 image 文件，加上一些个性化设置而生成。image 文件是通用的，一台机器的 image 文件拷贝到另一台机器，照样可以使用。

**image 文件生成的容器实例，本身也是一个文件，称为容器文件。**也就是说，一旦容器生成，就会同时存在两个文件： image 文件和容器文件。

#### 实例

从镜像仓库拉取 hello-world，默认镜像仓库地址：https://hub.docker.com/

`$ docker image pull library/hello-world`

docker image pull:从仓库抓取image文件

library/hello-world：image文件在仓库的位置，library是image文件的组，hello-world是image文件名

由于 Docker 官方提供的 image 文件，都放在[`library`](https://hub.docker.com/r/library/)组里面，所以它的是默认组，可以省略。

上面的命令也可以

`$ docker image pull hello-world`

抓取成功后本机就可以看到image 文件

`$ docker image ls `

运行image文件

`$ docker container run hello-world`

docker container run:从image文件生成一个container（容器）实例

说明：docker container run命令具有自动抓取功能，如果本地没有image文件，就会从仓库抓取，前面的 docker image pull非必须。

![image-20200925094831923](https://i.loli.net/2020/09/25/XoPfphTAmd9uWHl.png)

输出这段提示以后，`hello world`就会停止运行，容器自动终止。

有些容器不会自动终止，终止需要 docker container kill [容器ID]

后台运行的

`$ docker container run -it ubuntu /bin/bash`  :container可以省略

`$ docker container ls`：查看运行中的容器

$ docker container ls -a ：所有容器

#### image和container关系

- 要有Container首先要有Image，也就是说Container是通过image创建的。
- Container是在原先的Image之上新加的一层，称作Container layer，这一层是可读可写的（Image是只读的）。
- 在面向对象的编程语言中，有类跟对象的概念。类是抽象的，对象是类的具体实现。Image跟Container可以类比面向对象中的类跟对象，Image就相当于抽象的类，Container就相当于具体实例化的对象。
- Image跟Container的职责区别：Image负责APP的存储和分发，Container负责运行APP。

#### image和container常用命令

##### 拉取镜像

docker image pull  [镜像名称] ：拉取镜像

##### 查看镜像容器

docker image ls ：查看镜像

docker container ls或docker ps ：查看运行中的容器

docker container ls -a或docker ps -a：查看所有容器

##### 运行容器docker run 和start区别

docker container run [镜像名称]：运行镜像，每次运行都会生成一个新的容器（一般运行一次）

docker container start  [containerID]：启动已有的容器

docker logs [容器ID] ：查看日志

docker container cp ：`docker container cp`命令用于从正在运行的 Docker 容器里面，将文件拷贝到本机。下面是拷贝到当前目录的写法。

> ```bash
> $ docker container cp [containID]:[/path/to/file] .
> ```

docker run -t -i ubuntu /bin/bash

说明：

- ​         docker run：启动container，本地没有远程拉取
- ​         ubuntu：你想要启动的image
- ​         -t：进入终端
- ​          -i：获得一个交互式的连接，通过获取container的输入
- ​          /bin/bash：在container中启动一个bash shell

退出Ctrl+D 或exit

再次进入：1、启动  ： docker start|stop|restart  [容器ID或名称]

1）、docker attach [容器ID或名称]:这个退出，容器就关闭了

2）、docker exec -it crazy_lalande /bin/bash

​         docker exec -it：进入容器（运行中的）

​         crazy_lalande ：容器名

​         /bin/bash：**在container中启动一个bash shell**

##### 退出容器

退出容器： exit 或 CTRL+D

##### 删除容器

删除容器：  docker  container  rm [容器ID]

删除镜像  ： docker rmi [镜像名] 或docker image rm [镜像名]

1. `-f, -force`: 强制删除镜像，即便有容器引用该镜像；
2. `-no-prune`: 不要删除未带标签的父镜像；

##### docker stop 和docker kill 区别

docker stop，支持“优雅退出”。先发送SIGTERM信号，在一段时间之后（10s）再发送SIGKILL信号。Docker内部的应用程序可以接收SIGTERM信号，然后做一些“退出前工作”，比如保存状态、处理当前请求等。
docker kill，发送SIGKILL信号，应用程序直接退出。

## Dockerfile

### 制作自己的image

eg：

```bash
FROM node:8.4
COPY . /app
WORKDIR /app
RUN npm install --registry=https://registry.npm.taobao.org
EXPOSE 3000
```

含义

- `FROM node:8.4`：该 image 文件继承官方的 node image，冒号表示标签，这里标签是`8.4`，即8.4版本的 node。
- `COPY . /app`：将当前目录下的所有文件（除了`.dockerignore`排除的路径），都拷贝进入 image 文件的`/app`目录。
- `WORKDIR /app`：指定接下来的工作路径为`/app`。
- `RUN npm install`：在`/app`目录下，运行`npm install`命令安装依赖。注意，安装后所有的依赖，都将打包进入 image 文件。
- `EXPOSE 3000`：将容器 3000 端口暴露出来， 允许外部连接这个端口。

### 创建image文件

```bash
$ docker image build -t koa-demo .
# 或者
$ docker image build -t koa-demo:0.0.1 .
```

上面代码中，`-t`参数用来指定 image 文件的名字，后面还可以用冒号指定标签。如果不指定，默认的标签就是`latest`。最后的那个点表示 Dockerfile 文件所在的路径，上例是当前路径，所以是一个点。

### 生成容器

```bash
$ docker container run -p 8000:3000 -it koa-demo /bin/bash
# 或者
$ docker container run -p 8000:3000 -it koa-demo:0.0.1 /bin/bash
```

参数含义：

- `-p`参数：容器的 3000 端口映射到本机的 8000 端口。

- `-it`参数：容器的 Shell 映射到当前的 Shell，然后你在本机窗口输入的命令，就会传入容器。

- `koa-demo:0.0.1`：image 文件的名字（如果有标签，还需要提供标签，默认是 latest 标签）。

- `/bin/bash`：容器启动以后，内部第一个执行的命令。这里是启动 Bash，保证用户可以使用 Shell。

  

### CMD命令

```bash
FROM node:8.4
COPY . /app
WORKDIR /app
RUN npm install --registry=https://registry.npm.taobao.org
EXPOSE 3000
CMD node demos/01.js
```

最后一行`CMD node demos/01.js`，它表示容器启动后自动执行`node demos/01.js`

`RUN`命令在 image 文件的构建阶段执行，执行结果都会打包进入 image 文件；`CMD`命令则是在容器启动后执行

一个 Dockerfile 可以包含多个`RUN`命令，但是只能有一个`CMD`命令

注意，指定了`CMD`命令以后，`docker container run`命令就不能附加命令了（比如前面的`/bin/bash`），否则它会覆盖`CMD`命令。现在，启动容器可以使用下面的命令。

> ```bash
> $ docker container run --rm -p 8000:3000 -it koa-demo:0.0.1
> ```

### 发布image

首先，去 [hub.docker.com](https://hub.docker.com/) 或 [cloud.docker.com](https://cloud.docker.com/) 注册一个账户。然后，用下面的命令登录。

> ```bash
> $ docker login
> ```

接着，为本地的 image 标注用户名和版本。

> ```bash
> $ docker image tag [imageName] [username]/[repository]:[tag]
> # 实例
> $ docker image tag koa-demos:0.0.1 ruanyf/koa-demos:0.0.1
> ```

也可以不标注用户名，重新构建一下 image 文件。

> ```bash
> $ docker image build -t [username]/[repository]:[tag] .
> ```

最后，发布 image 文件。

> ```bash
> $ docker image push [username]/[repository]:[tag]
> ```

### dockerfile书写规范

指令忽略大小写，建议使用大写；每一行只支持一条指令，每条指令可以携带多个参数

### cmd和ENTRYPOINT的区别



### docker save与docker export的区别

 *注：用户既可以使用 docker load 来导入镜像存储文件到本地镜像库，也可以使用 docker import 来导入一个容器快照到本地镜像库。这两者的区别在于容器快照文件将丢弃所有的历史记录和元数据信息（即仅保存容器当时的快照状态），而镜像存储文件将保存完整记录，体积也要大。此外，从容器快照文件导入时可以重新指定标签等元数据信息。

#### docker save

 docker的命令行接口设计得很优雅，很多命令的帮助直接在后面加`--help`就可以查看。

 docker save的帮助如下：

```bash
>docker save --help
```

 从命令行帮助可以看出，docker save是用来将一个或多个image打包保存的工具。

 例如我们想将镜像库中的postgres和mongo打包，那么可以执行：

```
docker save -o images.tar postgres:9.6 mongo:3.4
```

 打包之后的`images.tar`包含`postgres:9.6`和`mongo:3.4`这两个镜像。

 虽然命令行参数要求指定image，实际上也可以对container进行打包，例如：

```bash
>docker ps



CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
3623943d369f        postgres:9.6        "docker-entrypoint..."   3 hours ago         Up 3 hours          5432/tcp            postgres
>docker save -o b.tar postgres
>docker save -o c.tar postgres:9.6
>ls -al
-rwxrwxrwx 1 root root 277886464 8月  26 14:40 b.tar
-rwxrwxrwx 1 root root 277886464 8月  26 14:41 c.tar
```

 通过以上命令可以看到，`b.tar`和`c.tar`是完全一模一样的。这说明，docker save如果指定的是container，docker save将保存的是容器背后的image。

 将打包后的镜像载入进来使用docker load，例如：

```
docker load -i images.tar
```

 上述命令将会把`postgres:9.6`和`mongo:3.4`载入进来，如果本地镜像库已经存在这两个镜像，将会被覆盖。

 docker save的应用场景是，如果你的应用是使用docker-compose.yml编排的多个镜像组合，但你要部署的客户服务器并不能连外网。这时，你可以使用docker save将用到的镜像打个包，然后拷贝到客户服务器上使用docker load载入。

#### docker export

 照例查看下docker export的帮助：

```
>docker export --help
```

 从帮助可以看出，docker export是用来将container的文件系统进行打包的。例如：

```
docker export -o postgres-export.tar postgres
```

 docker export需要指定container，不能像docker save那样指定image或container都可以。

 将打包的container载入进来使用docker import，例如：

```
docker import postgres-export.tar postgres:latest
```

 从上面的命令可以看出，docker import将container导入后会成为一个image，而不是恢复为一个container。

 另外一点是，docker import可以指定IMAGE[:TAG]，说明我们可以为镜像指定新名称。如果本地镜像库中已经存在同名的镜像，则原有镜像的名称将会被剥夺，赋给新的镜像。原有镜像将成为孤魂野鬼，只能通过IMAGE ID进行操作。

 docker export的应用场景主要用来制作基础镜像，比如你从一个ubuntu镜像启动一个容器，然后安装一些软件和进行一些设置后，使用docker export保存为一个基础镜像。然后，把这个镜像分发给其他人使用，比如作为基础的开发环境。

##### docker save和docker export的区别

 总结一下docker save和docker export的区别：

1. docker save保存的是镜像（image），docker export保存的是容器（container）；
2. docker load用来载入镜像包，docker import用来载入容器包，但两者都会恢复为镜像；
3. docker load不能对载入的镜像重命名，而docker import可以为镜像指定新名称。

## Docker-Compose

### 简介

Docker-Compose项目是Docker官方的开源项目，负责实现对Docker容器集群的快速编排。
Docker-Compose将所管理的容器分为三层，分别是工程（project），服务（service）以及容器（container）。Docker-Compose运行目录下的所有文件（docker-compose.yml，extends文件或环境变量文件等）组成一个工程，若无特殊指定工程名即为当前目录名。一个工程当中可包含多个服务，每个服务中定义了容器运行的镜像，参数，依赖。一个服务当中可包括多个容器实例，Docker-Compose并没有解决负载均衡的问题，因此需要借助其它工具实现服务发现及负载均衡。
Docker-Compose的工程配置文件默认为docker-compose.yml，可通过环境变量COMPOSE_FILE或-f参数自定义配置文件，其定义了多个有依赖关系的服务及每个服务运行的容器。
使用一个Dockerfile模板文件，可以让用户很方便的定义一个单独的应用容器。在工作中，经常会碰到需要多个容器相互配合来完成某项任务的情况。例如要实现一个Web项目，除了Web服务容器本身，往往还需要再加上后端的数据库服务容器，甚至还包括负载均衡容器等。
Compose允许用户通过一个单独的docker-compose.yml模板文件（YAML 格式）来定义一组相关联的应用容器为一个项目（project）。
Docker-Compose项目由Python编写，调用Docker服务提供的API来对容器进行管理。因此，只要所操作的平台支持Docker API，就可以在其上利用Compose来进行编排管理。
源码：https://github.com/docker/compose

##### 

通过浏览器访问web1，web2，web3服务:

```
http://127.0.0.1:6061
http://127.0.0.1:6062
http://127.0.0.1:6063
```

## 实例

### 自建wordpress容器

#### 方法一、自建 WordPress 容器

##### 新建一个目录

```bash
$ mkdir docker-demo && cd docker-demo
```

```bash
$ docker container run \
  --rm \
  --name wordpress \
  --volume "$PWD/":/var/www/html \
  php:5.6-apache
```

上面的命令基于`php`的 image 文件新建一个容器，并且运行该容器。`php`的标签是`5.6-apache`，说明装的是 PHP 5.6，并且自带 Apache 服务器。该命令的三个参数含义如下。

- `--rm`：停止运行后，自动删除容器文件。
- `--name wordpress`：容器的名字叫做`wordpress`。
- `--volume "$PWD/":/var/www/html`：将当前目录（`$PWD`）映射到容器的`/var/www/html`（Apache 对外访问的默认目录）。因此，当前目录的任何修改，都会反映到容器里面，进而被外部访问到。

运行上面的命令以后，如果一切正常，命令行会提示容器对外的 IP 地址，请记下这个地址，我们要用它来访问容器。我分配到的 IP 地址是 172.17.0.2。

打开浏览器，访问 172.17.0.2，你会看到下面的提示。

> ```bash
> Forbidden
> You don't have permission to access / on this server.
> ```

这是因为容器的`/var/www/html`目录（也就是本机的`docker-demo`目录）下面什么也没有，无法提供可以访问的内容。

请在本机的`docker-demo`目录下面，添加一个最简单的 PHP 文件`index.php`。

> ```bash
> <?php 
> phpinfo();
> ?>
> ```

保存以后，浏览器刷新`172.17.0.2`，应该就会看到熟悉的`phpinfo`页面了。

#### 拷贝 WordPress 安装包

```bash
$ wget https://cn.wordpress.org/wordpress-4.9.4-zh_CN.tar.gz
$ tar -xvf wordpress-4.9.4-zh_CN.tar.gz
```

解压以后，WordPress 的安装文件会在`docker-demo/wordpress`目录下。

这时浏览器访问`http://172.17.0.2/wordpress`，就能看到 WordPress 的安装提示了。

#### 官方的 MySQL 容器

```bash
$ docker container run \
  -d \
  --rm \
  --name wordpressdb \
  --env MYSQL_ROOT_PASSWORD=123456 \
  --env MYSQL_DATABASE=wordpress \
  mysql:5.7
```

上面的命令会基于 MySQL 的 image 文件（5.7版本）新建一个容器。该命令的五个命令行参数的含义如下。

- `-d`：容器启动后，在后台运行。
- `--rm`：容器终止运行后，自动删除容器文件。
- `--name wordpressdb`：容器的名字叫做`wordpressdb`
- `--env MYSQL_ROOT_PASSWORD=123456`：向容器进程传入一个环境变量`MYSQL_ROOT_PASSWORD`，该变量会被用作 MySQL 的根密码。
- `--env MYSQL_DATABASE=wordpress`：向容器进程传入一个环境变量`MYSQL_DATABASE`，容器里面的 MySQL 会根据该变量创建一个同名数据库（本例是`WordPress`）。

运行上面的命令以后，正常情况下，命令行会显示一行字符串，这是容器的 ID，表示已经新建成功了。

这时，使用下面的命令查看正在运行的容器，你应该看到`wordpress`和`wordpressdb`两个容器正在运行。

```bash
$ docker container ls
```

其中，`wordpressdb`是后台运行的，前台看不见它的输出，必须使用下面的命令查看。

> ```bash
> $ docker container logs wordpressdb
> ```

##### 定制 PHP 容器

```bash
$ docker container stop wordpress
```

停掉以后，由于`--rm`参数的作用，该容器文件会被自动删除。

然后，在`docker-demo`目录里面，新建一个`Dockerfile`文件，写入下面的内容。

```bash
FROM php:5.6-apache
RUN docker-php-ext-install mysqli
CMD apache2-foreground
```

上面代码的意思，就是在原来 PHP 的 image 基础上，安装`mysqli`的扩展。然后，启动 Apache。

基于这个 Dockerfile 文件，新建一个名为`phpwithmysql`的 image 文件。

> ```bash
> $ docker build -t phpwithmysql .
> ```

现在基于 phpwithmysql image，重新新建一个 WordPress 容器。

> ```bash
> $ docker container run \
>   --rm \
>   --name wordpress \
>   --volume "$PWD/":/var/www/html \
>   --link wordpressdb:mysql \
>   phpwithmysql
> ```

跟上一次相比，上面的命令多了一个参数`--link wordpressdb:mysql`，表示 WordPress 容器要连到`wordpressdb`容器，冒号表示该容器的别名是`mysql`。

这时还要改一下`wordpress`目录的权限，让容器可以将配置信息写入这个目录（容器内部写入的`/var/www/html`目录，会映射到这个目录）。

> ```bash
> $ chmod -R 777 wordpress
> ```

接着，回到浏览器的`http://172.17.0.2/wordpress`页面，点击"现在就开始！"按钮，开始安装。****

```bash
$ docker container stop wordpress wordpressdb
```

#### 方法二、采用官方的 WordPress 容器

首先，新建并启动 MySQL 容器。

```bash
 $ docker container run \
  -d \
  -p 127.0.0.2:8080:80 \
  --rm \
  --name wordpress \
  --env WORDPRESS_DB_PASSWORD=123456 \
  --link wordpressdb:mysql \
  --volume "$PWD/wordpress":/var/www/html \
  wordpress
```

```bash
$ docker container run \
  -d \
  --rm \
  --name wordpress \
  --env WORDPRESS_DB_PASSWORD=123456 \
  --link wordpressdb:mysql \
  wordpress
```

基于官方的 WordPress image，新建并启动 WordPress 容器。

上面命令指定`wordpress`容器在后台运行，导致前台看不见输出，使用下面的命令查出`wordpress`容器的 IP 地址。

```bash
$ docker container inspect wordpress
```

```bash
$ docker container stop wordpress wordpressdb
```

#### 方法三、采用 Docker Compose 工具

```bash
# 启动所有服务
$ docker-compose up
# 关闭所有服务
$ docker-compose stop
```

在`docker-demo`目录下，新建`docker-compose.yml`文件，写入下面的内容。

```bash
mysql:
    image: mysql:5.7
    environment:
     - MYSQL_ROOT_PASSWORD=123456
     - MYSQL_DATABASE=wordpress
web:
    image: wordpress
    links:
     - mysql
    environment:
     - WORDPRESS_DB_PASSWORD=123456
    ports:
     - "127.0.0.3:8080:80"
    working_dir: /var/www/html
    volumes:
     - wordpress:/var/www/html
```

```bash
$ docker-compose up
```

可以访问了

```bash
$ docker-compose stop
```

```bash
$ docker-compose rm
```

### docker部署springboot项目



## docker性能分析

 du -hs /var/lib/docker/ 命令查看磁盘使用情况

**docker system df**命令，类似于Linux上的**df**命令，用于查看Docker的磁盘使用情况:

**docker system prune**命令可以用于清理磁盘，删除关闭的容器、无用的数据卷和网络，以及dangling镜像(即无tag的镜像)。

 **docker system prune -a**命令清理得更加彻底，可以将没有容器使用Docker镜像都删掉。注意，这两个命令会把你暂时关闭的容器，以及暂时没有用到的Docker镜像都删掉了

## docker管理工具

Portainer 