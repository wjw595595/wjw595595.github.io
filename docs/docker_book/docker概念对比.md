Dockerfile中命令

docker导入导出

save和load

export 和import



对比

|                    | save                                                         | export                                                       |
| ------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 保存内容           | 镜像（image）                                                | 容器（container）                                            |
| 命令               | docker save [OPTIONS] IMAGE [IMAGE...]                       | docker export [OPTIONS] CONTAINER                            |
| eg                 | docker save -o nginx.tar nginx:latest                        | docker export -o nginx-test.tar nginx-test                   |
| 对应导入命令       | load载入镜像包                                               | import载入容器包                                             |
| 镜像是否可以重命名 | 不可以                                                       | 可以                                                         |
| 包大小             | 大于容器包                                                   | 容量小                                                       |
| 内容对比           | 包含镜像历史（可以回滚之前的层）docker images --tree（查看） | 丢失镜像所有的历史                                           |
| 使用场景           | 比如：纯备份，使用docker-compose编排的统一打包               | 制作基础镜像（容器中安装了新的软件应用等，启动容器后有变化） |
| 导入               | docker load -i xxxx.tar                                      | docker import - dockertest:1.0 ； **dockerservice:1.0** 是新镜像的名字，可以随意命名 |
| 备注               | 不需要                                                       | 导入的镜像必须带command  查看  docker ps  --no-trunc         |
|                    | docker run -p 8080:8080 --name=monitor-service -itd monitor-service:1.0 | docker run -p 8080:8080 --name=monitor-service -itd monitor-service:1.0  /bin/bash(这就是命令) |

