

```
#下载,解压安装
https://github.com/prometheus/node_exporter/releases/tag/v1.1.2
tar xvfz node_exporter-1.1.2.linux-amd64.tar.gz -C /usr/local/bin
cd /usr/local/bin
mv node_exporter-1.1.2.linux-amd64 node_exporter
```

创建用户组（也可以不创建）

```

sudo groupadd -r prometheus 
sudo useradd -r -g prometheus -s /sbin/nologin -M -c "prometheus Daemons" prometheus
```

//不创建组 就把user和group 去掉 ，注意端口冲突   netstat -an | grep 9100

查找pid：netstat -aon| findstr "9100"

pid对应的程序：tasklist|findstr "9100"

杀死：taskkill /f /t /im 程序名.exe

```cmd
#vim /etc/systemd/system/node_exporter.service
[Service]
User=node_exporter
Group=node_exporter
ExecStart=/usr/local/bin/node_exporter/node_exporter\
                  --web.listen-address=:9100\

[Install]
WantedBy=multi-user.target

[Unit]
Description=node_exporter
After=network.target

```



安装：
node exporter

官方文档：https://prometheus.io/docs/guides/node-exporter/

https://www.cnblogs.com/roger888/p/10535751.html

https://grafana.com/dashboards/8919

https://grafana.com/grafana/dashboards/11074

## 无代理

Linux无代理snmp： LM-SENSORS-MIB

常用的oid

https://blog.csdn.net/weixin_42506599/article/details/105569398

可以参考zabbix模板

https://www.cnblogs.com/hwlong/p/9291337.html