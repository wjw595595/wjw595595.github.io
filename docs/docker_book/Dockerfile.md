# Dockerfile详解

## 什么是dockerfile

Dockerfile是包含了组合 image命令的文本文件，Docker通过读取Dockerfile生成image

docker build 用于构建镜像  docker build -f   /opt/dockerfile

## dockerfile结构

Dockerfile 一般分为四部分：基础镜像信息、维护者信息、镜像操作指令和容器启动时执行指令，’#’ 为 Dockerfile 中的注释。

## dockerfile文件说明

Docker以从上到下的顺序运行Dockerfile的指令。为了指定基本映像，第一条指令必须是*FROM*。一个声明以`＃`字符开头则被视为注释。

```shell
docker build [OPTIONS] -f- PATH
```

### FROM

：指定基础镜像，必须为第一个命令 ，尽可能使用官方镜像作为基础镜像，推荐Alpine映像，因为它受到严格控制，大小很小(目前小于5mb)，同时仍然是一个完整的Linux发行版。如：jdk可以选  FROM openjdk:8-jdk-alpine

```shell
格式：
　　FROM <image>
　　FROM <image>:<tag>
　　FROM <image>@<digest>
示例：
　　FROM mysql:5.6
注：
　　tag或digest是可选的，如果不使用这两个值时，会使用latest版本的基础镜像
```





### MAINTAINER

: 维护者信息

```shell
格式：
    MAINTAINER <name>
示例：
    MAINTAINER Jasper Xu
    MAINTAINER sorex@163.com
    MAINTAINER Jasper Xu <sorex@163.com>
```

### RUN

：构建镜像时执行的命令

```shell
RUN用于在镜像容器中执行命令，其有以下两种命令执行方式：
shell执行
格式：
    RUN <command>
exec执行
格式：
    RUN ["executable", "param1", "param2"]
示例：
    RUN ["executable", "param1", "param2"]
    RUN apk update
    RUN ["/etc/execfile", "arg1", "arg1"]
注：　　RUN指令创建的中间镜像会被缓存，并会在下次构建中使用。如果不想使用这些缓存镜像，可以在构建时指定--no-cache参数，如：docker build --no-cache
```

### ADD

：将本地文件添加到容器中，tar类型文件会自动解压(网络压缩资源不会被解压)，可以访问网络资源，类似wget

```shell
格式：
    ADD <src>... <dest>
    ADD ["<src>",... "<dest>"] 用于支持包含空格的路径
示例：
    ADD hom* /mydir/          # 添加所有以"hom"开头的文件
    ADD hom?.txt /mydir/      # ? 替代一个单字符,例如："home.txt"
    ADD test relativeDir/     # 添加 "test" 到 `WORKDIR`/relativeDir/
    ADD test /absoluteDir/    # 添加 "test" 到 /absoluteDir/
```

### COPY

：功能类似ADD，但是是不会自动解压文件，也不能访问网络资源

### CMD

**构建容器后调用，也就是在容器启动时才进行调用**

```shell
格式：
    CMD ["executable","param1","param2"] (执行可执行文件，使用exec执行，推荐方式)
    CMD ["param1","param2"] (设置了ENTRYPOINT，则直接调用ENTRYPOINT添加参数：提供给ENTRYPOINT的默认参数)
    CMD command param1 param2 (执行shell内部命令，在/bin/sh 中执行)
示例：
    CMD echo "This is a test." | wc -
    CMD ["/usr/bin/wc","--help"]注： 　　CMD不同于RUN，CMD用于指定在容器启动时所要执行的命令，而RUN用于指定镜像构建时所要执行的命令。
```

### ENTRYPOINT

**：配置容器，使其可执行化。配合CMD可省去"application"，只使用参数。**

```shell
格式：
    ENTRYPOINT ["executable", "param1", "param2"] (可执行文件, 优先)
    ENTRYPOINT command param1 param2 (shell内部命令)
示例：
    FROM ubuntu
    ENTRYPOINT ["top", "-b"]
    CMD ["-c"]注：　　　ENTRYPOINT与CMD非常类似，不同的是通过docker run执行的命令不会覆盖ENTRYPOINT，而docker run命令中指定的任何参数，都会被当做参数再次传递给ENTRYPOINT。Dockerfile中只允许有一个ENTRYPOINT命令，多指定时会覆盖前面的设置，而只执行最后的ENTRYPOINT指令。
```

### **LABEL**

**用于为镜像添加元数据**

```shell
格式：
    LABEL <key>=<value> <key>=<value> <key>=<value> ...
示例：
　　LABEL version="1.0" description="这是一个Web服务器" by="IT笔录"
注：
　　使用LABEL指定元数据时，一条LABEL指定可以指定一或多条元数据，指定多条元数据时不同元数据之间通过空格分隔。推荐将所有的元数据通过一条LABEL指令指定，以免生成过多的中间镜像。
```

### **ENV**

**设置环境变量**

```shell
格式：
    ENV <key> <value>  #<key>之后的所有内容均会被视为其<value>的组成部分，因此，一次只能设置一个变量
    ENV <key>=<value> ...  #可以设置多个变量，每个变量为一个"<key>=<value>"的键值对，如果<key>中包含空格，可以使用\来进行转义，也可以通过""来进行标示；另外，反斜线也可以用于续行
示例：
    ENV myName John Doe
    ENV myDog Rex The Dog
    ENV myCat=fluffy
```

### **EXPOSE**

**指定于外界交互的端口**

```shell
格式：
    EXPOSE <port> [<port>...]
示例：
    EXPOSE 80 443
    EXPOSE 8080    EXPOSE 11211/tcp 11211/udp注：　　EXPOSE并不会让容器的端口访问到主机。要使其可访问，需要在docker run运行容器时通过-p来发布这些端口，或通过-P参数来发布EXPOSE导出的所有端口
```

### **VOLUME**

**用于指定持久化目录**

```shell
格式：
    VOLUME ["/path/to/dir"]
示例：
    VOLUME ["/data"]
    VOLUME ["/var/www", "/var/log/apache2", "/etc/apache2"注：　　一个卷可以存在于一个或多个容器的指定目录，该目录可以绕过联合文件系统，并具有以下功能：

1 卷可以容器间共享和重用
2 容器并不一定要和其它容器共享卷
3 修改卷后会立即生效
4 对卷的修改不会对镜像产生影响
5 卷会一直存在，直到没有任何容器在使用它
```

### **WORKDIR**

**工作目录，类似于cd命令**

```shell
格式：
    WORKDIR /path/to/workdir
示例：
    WORKDIR /a  (这时工作目录为/a)
    WORKDIR b  (这时工作目录为/a/b)
    WORKDIR c  (这时工作目录为/a/b/c)注：　　通过WORKDIR设置工作目录后，Dockerfile中其后的命令RUN、CMD、ENTRYPOINT、ADD、COPY等命令都会在该目录下执行。在使用docker run运行容器时，可以通过-w参数覆盖构建时所设置的工作目录。
```



### **USER**

**指定运行容器时的用户名或 UID，后续的 RUN 也会使用指定用户。使用USER指定用户时，可以使用用户名、UID或GID，或是两者的组合。当服务不需要管理员权限时，可以通过该命令指定运行用户。并且可以在之前创建所需要的用户**

```shell
格式:　　
USER user　　
USER user:group　　
USER uid　　
USER uid:gid　　
USER user:gid　　
USER uid:group
 示例：  USER www
 注：
　　使用USER指定用户后，Dockerfile中其后的命令RUN、CMD、ENTRYPOINT都将使用该用户。镜像构建完成后，通过docker run运行容器时，可以通过-u参数来覆盖所指定的用户。
```

### **ARG**

**用于指定传递给构建运行时的变量**

```shell
格式：
    ARG <name>[=<default value>]
示例：
    ARG site
    ARG build_user=www
```

### **ONBUILD**

**用于设置镜像触发器**

```shell
格式：　　ONBUILD [INSTRUCTION]
示例：
　　ONBUILD ADD . /app/src
　　ONBUILD RUN /usr/local/bin/python-build --dir /app/src
注：　　当所构建的镜像被用做其它镜像的基础镜像，该镜像中的触发器将会被钥触发
```



### 实例

```shell
# This my first nginx Dockerfile
# Version 1.0

# Base images 基础镜像
FROM centos

#MAINTAINER 维护者信息
MAINTAINER tianfeiyu 

#ENV 设置环境变量
ENV PATH /usr/local/nginx/sbin:$PATH

#ADD  文件放在当前目录下，拷过去会自动解压
ADD nginx-1.8.0.tar.gz /usr/local/  
ADD epel-release-latest-7.noarch.rpm /usr/local/  

#RUN 执行以下命令 
RUN rpm -ivh /usr/local/epel-release-latest-7.noarch.rpm
RUN yum install -y wget lftp gcc gcc-c++ make openssl-devel pcre-devel pcre && yum clean all
RUN useradd -s /sbin/nologin -M www

#WORKDIR 相当于cd
WORKDIR /usr/local/nginx-1.8.0 

RUN ./configure --prefix=/usr/local/nginx --user=www --group=www --with-http_ssl_module --with-pcre && make && make install

RUN echo "daemon off;" >> /etc/nginx.conf

#EXPOSE 映射端口
EXPOSE 80

#CMD 运行以下命令
CMD ["nginx"]
```



### 个别概念的区别

#### RUN和CMD

RUN命令是创建Docker镜像（image）的步骤，RUN命令对Docker容器（ container）造成的改变是会被反映到创建的Docker镜像上的。一个Dockerfile中可以有许多个RUN命令。

CMD
CMD命令是当Docker镜像被启动后Docker容器将会默认执行的命令。一个Dockerfile中只能有一个CMD命令。通过执行docker run imageimageother_command启动镜像可以重载CMD命令。

#### CMD和ENTRYPOINT

共同点：

1. **都可以指定shell或exec函数调用的方式执行命令；**
2. **当存在多个CMD指令或ENTRYPOINT指令时，只有最后一个生效；**

 **差异1：CMD指令指定的容器启动时命令可以被docker run指定的命令覆盖，而ENTRYPOINT指令指定的命令不能被覆盖，而是将docker run指定的参数当做ENTRYPOINT指定命令的参数。**

​    **差异2：CMD指令可以为ENTRYPOINT指令设置默认参数，而且可以被docker run指定的参数覆盖；**

参考：https://blog.csdn.net/wuce_bai/article/details/88997725

#### ADD和copy

ADD和COPY：

共同点：1、把本地文件拷贝到镜像，那么文件必须在上下文中，没有就会找不到路径

​               2、只复制目录中的内容，不包括目录本身 

区别：

COPY: **COPY <src> <dest>**

 如果仅仅是把本地的文件拷贝到容器镜像中，COPY 命令是最合适不过的

还支持通配符：

```
COPY check* /testdir/           # 拷贝所有 check 开头的文件
COPY check?.log /testdir/       # ? 是单个字符的占位符，比如匹配文件 check1.log
```



命令汇总：

```javascript
FROM: 指定基础镜像
RUN： 构建镜像过程中需要执行的命令。可以有多条。
CMD：添加启动容器时需要执行的命令。多条只有最后一条生效。可以在启动容器时被覆盖和修改。
ENTRYPOINT：同CMD，但这个一定会被执行，不会被覆盖修改。
LABEL ：为镜像添加对应的数据。
MLABELAINTAINER：表明镜像的作者。将被遗弃，被LABEL代替。
EXPOSE：设置对外暴露的端口。
ENV：设置执行命令时的环境变量，并且在构建完成后，仍然生效
ARG：设置只在构建过程中使用的环境变量，构建完成后，将消失
ADD：将本地文件或目录拷贝到镜像的文件系统中。能解压特定格式文件，能将URL作为要拷贝的文件
COPY：将本地文件或目录拷贝到镜像的文件系统中。
VOLUME：添加数据卷
USER：指定以哪个用户的名义执行RUN, CMD 和ENTRYPOINT等命令
WORKDIR：设置工作目录
ONBUILD：如果制作的镜像被另一个Dockerfile使用，将在那里被执行Docekrfile命令
STOPSIGNAL：设置容器退出时发出的关闭信号。
HEALTHCHECK：设置容器状态检查。
SHELL：更改执行shell命令的程序。Linux的默认shell是[“/bin/sh”, “-c”]，Windows的是[“cmd”, “/S”, “/C”]
```



![img](https://i.loli.net/2020/10/20/IAXmoH4cCMQwuL7.png)

## 参考：

dockerfile常用命令

https://docs.docker.com/engine/reference/builder/

dockerfile最佳实践

https://docs.docker.com/develop/develop-images/dockerfile_best-practices/