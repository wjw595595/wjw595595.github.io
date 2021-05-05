https://www.pianshen.com/article/1441964765/

下载

a. 进入Arangodb官网:https://www.arangodb.com/

选择社区版本

下载zip版本

安装教程

https://www.arangodb.com/docs/stable/installation-windows.html

在D:\GreenSoft\ArangoDB3-3.7.10\usr\bin  目录下运行

```
$arangod --install-service
```

![image-20210425133401041](https://i.loli.net/2021/04/25/NzmqVZgYcHwX5tS.png)

成功

 接着，在bin目录下点击arangod.exe,出现如下havefun！，说明服务开启。

http://127.0.0.1:8529/

默认用户root  密码空 

修改密码  左侧 user中

123456

导出:

```shell
#!/bin/sh 
arangodump --server.endpoint tcp://192.168.1.171:8902 --server.username root --server.password vangoo123 --server.database vangoo --output-directory /home/ago/dump-$(date "+%Y%m%d-%H:%M:%S")
find /home/ago/ -mtime +7 -name "dump-*" -exec rm -rf {} \; > /dev/null​
```

导入： 在D:\GreenSoft\ArangoDB3-3.7.10\usr\bin 执行

```shell
arangorestore --server.endpoint tcp://localhost:8529 --server.username root --server.password 123456 --server.database vangoo --input-directory E:\dump --overwrite true
```

