## 查看版本

cat /etc/redhat-release

## ifconfig安装包

yum install net-tools

## 挂载光盘镜像

1. mkdir /mnt/dvd
mount -o loop /root/CentOS-7-x86_64-Everything-1810.iso /mnt/dvd
2. 卸载： umount /mnt/cdrom 
3. http://mirrors.163.com/centos/7.8.2003/os/x86_64/Packages/ centos相关的包

**注意**： centos7 自带python2.7 不能卸载，yum是用python2,7写的 

## ssh连接自动断开

```shell
$ vi /etc/ssh/sshd_config
#找到
ClientAliveInterval 0
ClientAliveCountMax 3
#修改为
ClientAliveInterval 60
ClientAliveCountMax 5
#重启
systemctl restart sshd
```

## 更新yum源

centos7 yum makecache：更新缓存 报错
There are no enabled repos.

第一步： curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo

第二步：yum makecache  或yum update

## 压缩

zip -r file.zip  file/