
第一步：
vim catalina.sh
在 CLASSPATH 上添加
CATALINA_PID="CATALINA_BASE/tomcat.pid"
![title](https://i.loli.net/2020/07/28/QnxBeMwfORWZTcE.png)

## 一、添加配置文件
/etc/systemd/system/tomcat.service 

```language
[Unit]
Description=Tomcat
After=syslog.target network.target remote-fs.target nss-lookup.target

[Service]
Type=forking

Environment="JAVA_HOME=/usr/java/jdk1.8.0_191"

PIDFile=/usr/local/tomcat8/tomcat.pid
ExecStart=/usr/local/tomcat8/bin/startup.sh
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s QUIT $MAINPID
PrivateTmp=true

[Install]
WantedBy=multi-user.target

```



