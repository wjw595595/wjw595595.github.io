# centos安装

## centos7安装  

安装前要安装jdk
### 一、下载
http://mirror.bit.edu.cn/apache/zookeeper/ 下载稳定版
### 二、解压安装
 tar    -zxvf    apache-zookeeper-3.5.8-bin.tar.gz -C /usr/local

### 修改配置文件
cp xxx/zookeeper/conf/zoo_sample.cfg xxx/zookeeper/conf/zoo.cfg
### 启动zookeeper

/usr/local/zookeeper/bin/zkServer.sh
###设置开机启动
创建 /etc/systemd/system/zookeeper.service 文件，内容下
```language
[Unit]
Description=ZooKeeper Service
After=network.target
After=syslog.target

[Service]
Environment=ZOO_LOG_DIR=/var/log/zookeeper
SyslogIdentifier=zookeeper

Type=forking
User=root
Group=root
ExecStart=/usr/local/zookeeper/bin/zkServer.sh start /usr/local/zookeeper/conf/zoo.cfg
ExecStop=/usr/local/zookeeper/bin/zkServer.sh stop /usr/local/zookeeper/conf/zoo.cfg
ExecReload=/usr/local/zookeeper/bin/zkServer.sh restart /usr/local/zookeeper/conf/zoo.cfg


[Install]
WantedBy=default.target

```

启动命令：
systemctl daemon-reload
systemctl enable zookeeper
 systemctl start zookeeper
systemctl stop zookeeper
参考：https://www.pocketdigi.com/20180131/1593.html
### 常见错误
1、Zookeeper JAVA_HOME is not set and java could not be found in PATH
修改zkEnv.sh文件 增加JAVA_HOME 
解决：进入Zookeeper的bin目录下，修改zkEnv.sh文件
添加java路径：
JAVA_HOME="/usr/java/jdk1.8.0_191"
![title](https://i.loli.net/2020/07/27/wyfGcOYqQXnl9xA.png)



