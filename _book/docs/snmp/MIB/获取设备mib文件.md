## 命令

snmpwalk -v 1/2c -c community IP .1>IP.mib

- community ：团体名
- IP ：ip地址
- .1 查询所有
- IP.mib ：输出到的文件名
- -Cc：去掉重复

eg：

```shell
snmpwalk -v2c -c public 192.168.210.254 .1 > /etc/telegraf/192.168.210.254.mib
#-Cc：去掉重复
snmpwalk -v2c -c   public -Cc 192.168.210.254 .1 >/etc/telegraf/192.168.210.254.mib
```

**注意**:一般来说使用snmpwalk -v 1/2c -c community IP .1>IP.mib命令采集设备MIB信息后，文件IP.mib应该包含该设备的所有MIB信息,

但是从实际反应的情况来看，某些设备产商由于SNMP实现支持不是很标准，故存在只能获取到公有MIB信息（即1.3.6.1.2.1开头的信息）的情况，此时请再使用命令snmpwalk -v 1/2c -c community IP .1.3.6.1.4.1>IP.private.mib采集设备私有MIB信息，并和前面采集到的IP.mib一同发给研发。

```
#两个都要
snmpwalk -v 1/2c -c community IP .1.3.6.1.4.1>IP.private.mib
```

小结：所谓MIB信息的完整性，即判断snmpwalk命令输出的文件信息中是否包含iso.3.6.1.2.1开头的OID信息（公有MIB信息），又是否包含iso.3.6.1.4.1开头的OID信息（私有MIB信息），甚至还可能包含iso.3.6.1.6.1开头的OID信息（该部分信息可有可无，目前暂时未用到）