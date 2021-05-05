第一步：复制相关东西

第二步：配置dockerfile

#基础镜像使用centos
FROM centos
#维护者信息
MAINTAINER wjw<wjw_595@126.com>
#将jdk包导入
ADD jdk-8u191-linux-x64.tar.gz /home
#将项目包导入
ADD monitorServices-1.0.0-SNAPSHOT.jar /home
#配置文件导入
COPY config /home
#依赖包导入
COPY lib /home

#设置工作空间
ENV WORKPATH /home/
WORKDIR $WORKPATH

#环境变量配置
ENV JAVA_HOME /home/jdk1.8.0_191
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV PATH $PATH:$JAVA_HOME/bin

#暴露端口
EXPOSE 8080

#执行启动指令
CMD java -jar -Dloader.path=lib,config  /home/monitorServices-1.0.0-SNAPSHOT.jar


第三步：制作镜像
docker build -f Dockerfile -t monitor-service:1.0 .

-f：是文件名，默认 dockerfile
-t：镜像名

第四部：保存镜像
docker save -o monitor-service.tar monitor-service

导入：docker load -i monitor-service.tar

第五步：运行容器 （可以用docker-compose 编排）

docker run -p 8080:8080 --name=monitor-service -itd monitor-service:1.0 (有CMD的就不能有参数/bin/bash)
（注意：docker run命令如果指定了参数会把CMD里的参数覆盖  /bin/bash  就是参数
一个Dockerfile仅仅最后一个CMD起作用
）
docker exec -it monitor-service /bin/bash

netstat -ntlp

#导出容器
docker export -o monitor-service.tar monitor-service

#导入镜像 (import 可以重命名)

docker import monitor-service.tar monitor-service:latest

注意：运行导入的镜像的时候必须带command，否则启动报如下错误
Error response from daemon: No command specified
具体的command需要在导出容器的时候通过docker ps查看到
docker ps  --no-trunc 

vue：
docker build -f Dockerfile -t monitor-web:1.0 .
docker run -p 8086:8086 --name=monitor-web -itd monitor-web:1.0

批量导入镜像：
ll *.tar|awk '{print $NF}'|sed -r 's#(.*)#docker load -i \1#' |bash