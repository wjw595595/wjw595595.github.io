# CentOS7中使用iptables

## 1、关闭firewall：

## 2、安装iptables防火墙

#安装
yum install iptables-services 

编辑防火墙配置文件

vi /etc/sysconfig/iptables

## 关闭SELINUX

vi /etc/selinux/config

#SELINUX=enforcing #注释掉
#SELINUXTYPE=targeted #注释掉
SELINUX=disabled #增加

setenforce 0 *#使配置立即生效*