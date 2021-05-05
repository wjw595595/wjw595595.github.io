本文安装环境CentOS7.x，版本Grafana-5.4.3

Web项目中我使用iframe直接嵌套进去的
![grafana.png](0)

但是浏览器缓存清除了或者session失效了，每次进入Web页面看Grafana的时候就需要重新登录，在官方社区查找，并没有太好的执行办法，最后决定把Grafana设置成匿名登录：

修改/etc/grafana/grafana.ini目录下的默认配置文件内容：
![grafana2.png](1)


然后重启grafana服务（systemctl restart grafana-server）就可以。
![title](../../.local/static/2020/3/6/1586573119540.1586573119672.png)