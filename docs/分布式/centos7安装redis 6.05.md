yum -y install gcc

下载安装包
tar -zxvf xxx.tar.gz

解压目录下：
编译安装
make PREFIX=/usr/local/redis install

### 修改配置
拷贝 redis.conf  到 bin目录
将 bind 127.0.0.1 ::1 这一行注释掉。
daemonize yes  #后台运行
修改办法：protected-mode no

启动
[root@mmjredis bin]# ./redis-server redis.conf
./redis-cli -h 127.0.0.1 -p 6379

### 远程访问

### 开机启动：
vi   /etc/systemd/system/multi-user.target.wants/redis.service
```language
[Unit]
Description=redis-server
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/redis/bin/redis-server   /usr/local/redis/bin/redis.conf
PrivateTmp=true

[Install]
WantedBy=multi-user.target


```

各项参数说明：
　　Description:描述服务
　　After:描述服务在哪些基础服务启动后再启动
　　[Service]服务运行参数的设置
　　Type=forking是最简单和速度最快的选择
　　ExecStart为启动服务的具体运行命令
　　ExecReload为重启命令
　　ExecStop为停止命令
　　PrivateTmp=True表示给服务分配独立的临时空间
　　注意：[Service]的启动、重启、停止命令全部要求使用绝对路径
　　[Install]运行级别下服务安装的相关设置，可设置为多用户，即系统运行级别为3
详细说明请参考systemd.service 中文手册网址：http://www.jinbuguo.com/systemd/systemd.service.html


### 常见报错：
```language
             serverLog(LL_NOTICE,"The server is now ready to accept connections at %s", server.unixsocket);
                                                                                              ^
server.c:5103:19: error: ‘struct redisServer’ has no member named ‘supervised_mode’
         if (server.supervised_mode == SUPERVISED_SYSTEMD) {
                   ^
server.c:5104:24: error: ‘struct redisServer’ has no member named ‘masterhost’
             if (!server.masterhost) {
                        ^
server.c:5117:15: error: ‘struct redisServer’ has no member named ‘maxmemory’
     if (server.maxmemory > 0 && server.maxmemory < 1024*1024) {
               ^
server.c:5117:39: error: ‘struct redisServer’ has no member named ‘maxmemory’
     if (server.maxmemory > 0 && server.maxmemory < 1024*1024) {
                                       ^
server.c:5118:176: error: ‘struct redisServer’ has no member named ‘maxmemory’
         serverLog(LL_WARNING,"WARNING: You specified a maxmemory value that is less than 1MB (current value is %llu bytes). Are you sure this is what you really want?", server.maxmemory);
                                                                                                                                                                                ^
server.c: In function ‘hasActiveChildProcess’:
server.c:1476:1: warning: control reaches end of non-void function [-Wreturn-type]
 }
 ^
server.c: In function ‘allPersistenceDisabled’:
server.c:1482:1: warning: control reaches end of non-void function [-Wreturn-type]
 }
 ^
server.c: In function ‘writeCommandsDeniedByDiskError’:
server.c:3747:1: warning: control reaches end of non-void function [-Wreturn-type]
 }
 ^
server.c: In function ‘iAmMaster’:
server.c:4914:1: warning: control reaches end of non-void function [-Wreturn-type]
 }
 ^
make[1]: *** [server.o] Error 1
make[1]: Leaving directory `/usr/src/redis-6.0.1/src'
make: *** [install] Error 2

```

```language
# 查看gcc版本是否在5.3以上，centos7.6默认安装4.8.5
gcc -v
# 升级gcc到5.3及以上,如下：
升级到gcc 9.3：
yum -y install centos-release-scl
yum -y install devtoolset-9-gcc devtoolset-9-gcc-c++ devtoolset-9-binutils
scl enable devtoolset-9 bash
需要注意的是scl命令启用只是临时的，退出shell或重启就会恢复原系统gcc版本。
如果要长期使用gcc 9.3的话：
 
echo "source /opt/rh/devtoolset-9/enable" >>/etc/profile
这样退出shell重新打开就是新版的gcc了
以下其他版本同理，修改devtoolset版本号即可。
```




https://blog.csdn.net/weidu01/article/details/105946606/