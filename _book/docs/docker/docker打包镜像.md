# 打包tar文件

## 命令

```shell
#打包镜像
docker save -o xxx.tar 镜像名
#加载镜像
docker load -i xxx.tar

```

删除镜像

```shell
docker rmi [image]
#或
docker image rm [image]
```

