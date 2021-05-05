

## 下载安装JRebel

IDEA 依次打开 File | Settings | Plugins → 搜索JRebel进行安装并重启IDEA

## 破解服务器

https://github.com/ilanyu/ReverseProxy/releases/latest

```cmd
docker pull ilanyu``/golang-reverseproxy
docker run --name jrebel -d --restart=always -p 7777:8888 ilanyu/golang-reverseproxy
//更新docker
docker container update --restart=always 容器名
```

## 生成guid

https://www.guidgen.com/

激活地址

 http://1.116.208.30:7777/{ GUID }

help->jrebel激活

激活后一定要手动切换到离线模式，可离线180天，可随时重新点下“Renew Offline Seat”刷新激活周期，180天后激活状态会重新刷新 。

![img](https://img.jbzj.com/file_images/article/201911/20191120151220102.jpg)

启动之前需要添加rebel.xml（你要热部署哪个项目就直接勾选，会自动为你进行配置，rebel.xml里默认配置了两个路径，作用为监控变化）

![img](https://img.jbzj.com/file_images/article/201911/20191120151221105.jpg)

![img](https://img.jbzj.com/file_images/article/201911/20191120151222108.jpg)

![img](https://img.jbzj.com/file_images/article/201911/20191120151222109.jpg)

Shift+Ctrl+Alt+/

![img](https://img.jbzj.com/file_images/article/201911/20191120151222110.jpg)

![img](https://img.jbzj.com/file_images/article/201911/20191120151222111.jpg)

 Ctrl + Shift + F9

![img](https://img.jbzj.com/file_images/article/201911/20191120151223112.jpg)