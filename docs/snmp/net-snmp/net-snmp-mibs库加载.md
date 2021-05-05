## mib默认加载路径

```
1. $HOME/.snmp/mibs
2. /usr/local/share/snmp/mibs

#查看
[root@localhost ~]# net-snmp-config --default-mibdirs
/root/.snmp/mibs:/usr/share/snmp/mibs
```



修改默认路径

1.如果系统安装了net-snmp-config:
\# net-snmp-config --default-mibdirs



net-snmp-config --snmpconfpath

显示;snmp.conf 的路径

net-snmp 正确加载mib （如果不能正确加载mib文件的话，就会出现"Unknown Object Identifier"这样的错误。）

+ 放在 mibs路径下（/root/.snmp/mibs:/usr/share/snmp/mibs ：两个路径一个）
+ 

https://my.oschina.net/u/4295888/blog/3340243

如果要net-snmp自动加载我们下载的新MIB文件，有两种方法:

## 方法一: 放到snmp.conf中。

用 net-snmp-config --snmpconfpath可以确定snmp.conf文件的位置
[root@Kickstart-O ~]# net-snmp-config --snmpconfpath
/etc/snmp:/usr/share/snmp:/usr/lib/snmp:/root/.snmp:/var/net-snmp
将所要加载的MIB的Module名称加到snmpd.conf中，如下例：
mibs +CISCO-RHINO-MIB
mibs +SOME-OTHER-SPIFFY-MIB
如果图省事也可以这样，当然我们不建议这样。
mibs +ALL
因为这样有可能会提示如下错误
\# snmpwalk -v2c public 192.168.1.100
Warning: Module MAU-MIB was in /usr/share/snmp/mibs//DOT3-MAU-MIB.txt now is /usr/share/snmp/mibs//RFC2668-MIB.txt
Warning: Module DISMAN-EVENT-MIB was in /usr/share/snmp/mibs//EVENT-MIB.txt now is /usr/share/snmp/mibs//DISMAN-EVENT-MIB.txt
Warning: Module P-BRIDGE-MIB was in /usr/share/snmp/mibs//P-BRIDGE-MIB.txt now is /usr/share/snmp/mibs//P-BRIDGE.txt
可以将标准错误文件转向来屏蔽这些警告信息
\# snmpwalk -v2c public 192.168.1.100 2>/dev/null
SNMPv2-MIB::sysDescr.0 = STRING: Linux server1 2.4.34-pre2 #170 Fri Sep 15 20:10:21 CEST 2006 mips
SNMPv2-MIB::sysObjectID.0 = OID: NET-SNMP-TC::linux
DISMAN-EVENT-MIB::sysUpTimeInstance = Timeticks: (706980) 1:57:49.80

## 方法二: 使用系统变量(定义MIBS变量)

\# MIBS=+CISCO-RHINO-MIB:SOME-OTHER-SPIFFY-MIB
\# export MIBS #导入MIBS