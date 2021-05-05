## 问题一

mysql too many connections 解决方法



```
mysql -u root -p 进入mysql
#查看连接数
show processlist; 
#查看连接数，可以发现有很多连接处于sleep状态，这些其实是暂时没有用的，所以可以kill掉
show variables like "max_connections"; 
#修改最大连接数
set GLOBAL max_connections=1000; 
#这个数值指的是mysql在关闭一个非交互的连接之前要等待的秒数，默认是28800s
show global variables like 'wait_timeout'; 
#修改这个数值，这里可以随意，最好控制在几分钟内 
set global wait_timeout=300; 

set global interactive_timeout=500; 

修改这个数值，表示mysql在关闭一个连接之前要等待的秒数，至此可以让mysql自动关闭那些没用的连接，但要注意的是，正在使用的连接到了时间也会被关闭，因此这个时间值要合适

批量kill之前没用的sleep连接，在网上搜索的方法对我都不奏效，因此只好使用最笨的办法，一个一个kill

8、select concat('KILL ',id,';') from information_schema.processlist where user='root'; 先把要kill的连接id都查询出来

然后用列模式 kill 掉


```

