# docker安装mysql

## 拉取镜像

 `docker pull mysql:5.7

## 运行

```shell
$ mkdir /usr/local/mysql
$ cd /usr/local/mysql
$ docker run -p 3306:3306 --name mysql57 --restart=always -v $PWD/conf:/etc/mysql/conf.d -v $PWD/logs:/logs -v $PWD/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7
```

- **-p 3306:3306**：将容器的 3306 端口映射到主机的 3306 端口。
- **-v -v $PWD/conf:/etc/mysql/conf.d**：将主机当前目录下的 conf/my.cnf 挂载到容器的 /etc/mysql/my.cnf。
- **-v $PWD/logs:/logs**：将主机当前目录下的 logs 目录挂载到容器的 /logs。
- **-v $PWD/data:/var/lib/mysql** ：将主机当前目录下的data目录挂载到容器的 /var/lib/mysql 。
- **-e MYSQL_ROOT_PASSWORD=123456：**初始化 root 用户的密码。
- --restart=always 随docker启动一块启动

## trouble

连接不上

```shell
$docker exec -it 62349aa31687 /bin/bash
mysql -uroot -p
#授权
mysql> GRANT ALL ON *.* TO 'root'@'%';
#刷新权限
mysql> flush privileges;
#更新加密规则：
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER;
#更新root用户
mysql> ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';
#刷新权限
mysql> flush privileges;
```

