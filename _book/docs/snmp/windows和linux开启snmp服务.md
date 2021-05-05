# 开启snmp服务

## linux开启

安装net-snmp

```
yum install net-snmp net-snmp-utils  

```

修改配置

```shell
 #vim /etc/snmp/snmpd.conf
  
 com2sec notConfigUser default（吮许那台主机采集数据） public(共同体名字)
 group notConfigGroup v1 notConfigUser
 group notConfigGroup v2c notConfigUser
 view systemview included .1
 access notConfigGroup "" any noauth exact systemview none none
 syslocation Unknown (edit /etc/snmp/snmpd.conf)
 syscontact Root wjw@163.com
```

重启snmpd服务

```shell
systemctl start snmpd
```

检查

```
#lsof -i :161
#netstat -nlup | grep ":161" 
或
#netstat -anp |grep snmpd
```

## windows打开

services.msc 中 snmp -->右键属性 -》安全配置团体名 和访问的地址