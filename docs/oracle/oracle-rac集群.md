# 规划

## 配置规划

系统安装规划:
ORACLE_BASE=/data/oracle
ORACLE_HOME=/data/oracle/product/11.2.0/db_1
DB_NAME=orcl
ORACLE_SID=orcl/
TNS_ADMIN=$ORACLE_HOME/network/admin
ORACLE管理账户口令：oracle
数据库存放位置=ASM
是否使用归档方式运行数据库=ARCHIVED
备份方式说明：RMAN

## 网络规划

物理机创建网络： 物理网卡（物理网口）-创建虚拟交换机（选择上行链路）--添加端口组

在物理机：192.168.210.236创建 vm-rac端口组

虚机通过esxi创建虚拟网络vm-rac



| eth0 |      |      |      |
| ---- | ---- | ---- | ---- |
|      |      |      |      |
|      |      |      |      |
|      |      |      |      |
|      |      |      |      |

公网、vip、scan 一个网段，私网只要能相互ping通就可以

public-ip：

vip

scan

private-ip

