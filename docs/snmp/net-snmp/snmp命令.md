## snmp命令

snmptable -v 2c -c public 192.168.210.254 1.3.6.1.2.1.2.2

snmpwalk -v 2c -c public 192.168.210.254 1.3.6.1.2.1.2.2.1.3

http://www.h3c.com/cn/d*201109/725296*30005_0.htm

## 交换机配置SNMP

配置

进入系统模式

system-view

启用：snmp-agent

关闭：undo snmp-agent

第一步连接h3c三层交换机

第二步进入特权模式

\#system

第三步增加一个snmp团体名称

\#snmp-agent community read community-name   # read 为只读模式，write 为读写模式

\#display snmp community         #查看当前团体名称

\#undo snmp-agent community read community-name 