https://blog.csdn.net/cfm_gavin/article/details/88543438

## 防火墙原因

CentOS7有很多CentOS 6中的常用服务发生了变化。首当其冲 防火墙iptables被firewalld取代。这里有个最大的问题是，firewalld 放行端口后 服务任然不能外网访问，因为要添加端口对应的服务到firewalld中，那么docker容器中的各个映射访问的端口就没法玩，docker容器映射出的端口 服务如何添加？非docker安装，如ftp,那么放行ftp端口后还须：

firewall-cmd --add-service=ftp // 即时放行了对应端口，无此步任无法访问，除非禁用 firewall

重新载入

firewall-cmd --reload // 方可生效，ftp 客户端才能使用

由此 采用systemctl关闭firewalld，开启iptables。

### 1.关闭firewalld

```shell
[root@~]# systemctl stop firewalld
[root@~]# systemctl disable firewalld
[root@~]# systemctl status firewalld
firewalld.service - firewalld - dynamic firewall daemon

Loaded: loaded (/usr/lib/systemd/system/firewalld.service; disabled)

Active: inactive (dead)
....
```



### 2.开启iptables

首先安装iptables：

```shell
[root@~]#yum install -y iptables-services
[root@~]# systemctl enable iptables
ln -s '/usr/lib/systemd/system/iptables.service' '/etc/systemd/system/basic.target.wants/iptables.service'

[root@~]# systemctl start iptables
[root@~]# systemctl status iptables
iptables.service - IPv4 firewall with iptables

Loaded: loaded (/usr/lib/systemd/system/iptables.service; enabled)

Active: active (exited) since Fri 2016-02-26 13:54:45 UTC; 6s ago

Process: 55539 ExecStart=/usr/libexec/iptables/iptables.init start (code=exited, status=0/SUCCESS)

Main PID: 55539 (code=exited, status=0/SUCCESS)

Feb 26 13:54:45 hwcentos70-01 iptables.init[55539]: iptables: Applying firewall rules: [ OK ]

Feb 26 13:54:45 hwcentos70-01 systemd[1]: Started IPv4 firewall with iptables.
```

### 此时iptables的命令都可以使用了：

```shell
[root@~]# iptables -L
Chain INPUT (policy ACCEPT)

target prot opt source destination

Chain FORWARD (policy ACCEPT)

target prot opt source destination

Chain OUTPUT (policy ACCEPT)

target prot opt source destination

[root@~]# service iptables save
iptables: Saving firewall rules to /etc/sysconfig/iptables:[ OK ]
```

其他参考

docker 端口映射 及外部无法访问问题
https://www.cnblogs.com/zl1991/p/10531726.html