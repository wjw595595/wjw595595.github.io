容器卷

参考

https://www.cnblogs.com/edisonchou/p/docker_volumes_introduction.html

vlolume的基本使用

1、管理卷

```shell
# docker volume create volume-test // 创建一个自定义容器卷
# docker volume ls // 查看所有容器卷
# docker volume inspect volume-test // 查看指定容器卷详情信息
```

![image-20201026142933023](https://i.loli.net/2020/10/26/qHCEfcZ1Vi6Pdsw.png)

2、三种数据持久化方式

数据从宿主机挂载到容器

1）、volumes（自定义容器卷）：默认位于/var/lib/docker/volumes 目录中（最常用方式）

2)、bind mounts：可以存储在宿主机系统的任意位置（常用）：劣势：在不同的操作系统不可以移植，不同操作系统目录结构不一样，（不能出现在Dockerfile中，出现dockerfile就不可移植）

3）、tmpfs：挂载存储在宿主机系统的内存中，不会写入宿主机的文件系统（不会用）

![image-20201026150410822](https://i.loli.net/2020/10/26/Ji49TX2fM1Qxt8W.png)





**docker commit 3bd0eef03413 demo：v1.3** 提交你刚才修改的镜像，新的镜像名称为demo，版本为v1.3



