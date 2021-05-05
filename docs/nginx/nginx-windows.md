## 安装

http://nginx.org/en/download.html

解压

点击 nginx.exe 

访问：80 端口

## 修改配置文件

conf/nginx.conf，修改默认端口

```
start nginx
```

![img](https://i.loli.net/2021/04/22/oWrXz2fFc1TmjeQ.png)

## 关闭

(1)输入nginx命令  nginx -s stop(快速停止nginx)  或  nginx -s quit(完整有序的停止nginx)

(2)使用taskkill  taskkill /f /t /im nginx.exe   查找进程 tasklist /fi "imagename eq nginx.exe"

## 负载均衡

```
nginx -s reload
```

```
start nginx
./nginx.exe
./nginx

#重新启动
./nginx -s reload
停止
./nginx -s stop
# 或者
./nginx -s quit
#打开日志
./nginx -s reopen
#版本
nginx -v

```

https://blog.csdn.net/zjsfdx/article/details/89787462 nginx 日志访问

https://www.jb51.net/article/92832.htm