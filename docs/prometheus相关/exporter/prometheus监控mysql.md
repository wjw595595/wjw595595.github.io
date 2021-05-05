# 监控mysql

## 环境

插件：mysql_exporter

版本： 0.12.1

官方地址：https://github.com/prometheus/mysqld_exporter

grafana模板：https://grafana.com/dashboards/7362

数据库版本：5.6以上

## 安装

### 压缩包安装

#### 1、安装包上传mysql服务器，解压到指定目录

```shell
tar zxvf mysqld_exporter-0.12.1.linux-amd64.tar.gz -C /usr/local
```

#### 2、重命名

```
cd /usr/local
mv mysqld_exporter-0.12.1.linux-amd64 /usr/local/mysql_exporter
```

#### 3、登录mysql 创建账户

```shell
# 本地访问
mysql> CREATE USER 'exporter'@'localhost' IDENTIFIED BY 'XXXXXXXX';
#或远程访问
mysql> CREATE USER 'exporter'@'%' IDENTIFIED BY 'XXXXXXXX';

授权：
# 可查看主从运行情况查看线程，及所有数据库。
mysql> GRANT PROCESS, REPLICATION CLIENT, SELECT ON *.* TO 'exporter'@'localhost';
#或远程
# 可查看主从运行情况查看线程，及所有数据库。
mysql> GRANT PROCESS, REPLICATION CLIENT, SELECT ON *.* TO 'exporter'@'%';

#刷新数据库
mysql> flush privileges;
```

#### 4、创建配置文件 

```
vim /usr/local/mysql_exporter/my.cnf
#内容
[client]
host=localhost
port=3306
user=exporter
password=123456
```

#### 5、启动exporter

```
./mysqld_exporter --config.my-cnf=my.cnf
```

```shell
#参数说明
常用参数：
# 选择采集innodb
--collect.info_schema.innodb_cmp
# innodb存储引擎状态
--collect.engine_innodb_status
# 指定配置文件
--config.my-cnf="my.cnf"
参考：
https://github.com/prometheus/mysqld_exporter
```

#### 6、添加系统服务

```
vi /usr/lib/systemd/system/mysql_exporter.service
```

```shell
#内容
[Unit]
Description=mysql_exporter
After=network.target
[Service]
Type=simple
User=mysql
Environment=DATA_SOURCE_NAME=exporter:123456@(localhost:3306)/
ExecStart=/usr/local/mysql_exporter/mysqld_exporter --web.listen-address=0.0.0.0:9104
  --config.my-cnf /usr/local/mysql_exporter/my.cnf \
  --collect.slave_status \
  --collect.slave_hosts \
  --log.level=error \
  --collect.info_schema.processlist \
  --collect.info_schema.innodb_metrics \
  --collect.info_schema.innodb_tablespaces \
  --collect.info_schema.innodb_cmp \
  --collect.info_schema.innodb_cmpmem
Restart=on-failure

[Install]
WantedBy=multi-user.target

```

```shell
#简洁版
[Unit]
Description=mysql_exporter

[Service]
Restart=on-failure
ExecStart=/usr/local/mysql_exporter/mysqld_exporter --config.my-cnf /usr/local/mysql_exporter/my.cnf

[Install]
WantedBy=multi-user.target
```

#### 7、启动

#chown -R root:root /usr/lib/systemd/system/mysql_exporter.service
chmod 644 /usr/lib/systemd/system/mysql_exporter.service
systemctl daemon-reload
systemctl enable mysqld_exporter.service
systemctl start mysqld_exporter.service

#### 8、查看捕获的mysql数据

http://192.168.210.60:9104/metrics

```shell
# HELP go_gc_duration_seconds A summary of the GC invocation durations.
# TYPE go_gc_duration_seconds summary
go_gc_duration_seconds{quantile="0"} 2.8648e-05
go_gc_duration_seconds{quantile="0.25"} 8.0458e-05
go_gc_duration_seconds{quantile="0.5"} 0.000156041
go_gc_duration_seconds{quantile="0.75"} 0.000201022
go_gc_duration_seconds{quantile="1"} 0.000706424
go_gc_duration_seconds_sum 0.76849535
go_gc_duration_seconds_count 4807
# HELP go_goroutines Number of goroutines that currently exist.
# TYPE go_goroutines gauge
go_goroutines 10
# HELP go_info Information about the Go environment.
# TYPE go_info gauge
go_info{version="go1.12.7"} 1
# HELP go_memstats_alloc_bytes Number of bytes allocated and still in use.
# TYPE go_memstats_alloc_bytes gauge
go_memstats_alloc_bytes 3.24544e+06
# HELP go_memstats_alloc_bytes_total Total number of bytes allocated, even if freed.
# TYPE go_memstats_alloc_bytes_total counter
go_memstats_alloc_bytes_total 1.3871545528e+10
# HELP go_memstats_buck_hash_sys_bytes Number of bytes used by the profiling bucket hash table.
# TYPE go_memstats_buck_hash_sys_bytes gauge
go_memstats_buck_hash_sys_bytes 1.584137e+06
......
......
......
mysql_info_schema_innodb_cmpmem_relocation_time_seconds_total{buffer_pool="0",page_size="1024"} 0
mysql_info_schema_innodb_cmpmem_relocation_time_seconds_total{buffer_pool="0",page_size="16384"} 0
mysql_info_schema_innodb_cmpmem_relocation_time_seconds_total{buffer_pool="0",page_size="2048"} 0
mysql_info_schema_innodb_cmpmem_relocation_time_seconds_total{buffer_pool="0",page_size="4096"} 0
mysql_info_schema_innodb_cmpmem_relocation_time_seconds_total{buffer_pool="0",page_size="8192"} 0
# HELP mysql_up Whether the MySQL server is up.
# TYPE mysql_up gauge
mysql_up 1
# HELP mysql_version_info MySQL version and distribution.
# TYPE mysql_version_info gauge
mysql_version_info{innodb_version="5.6.47",version="5.6.47",version_comment="MySQL Community Server (GPL)"} 1
# HELP mysqld_exporter_build_info A metric with a constant '1' value labeled by version, revision, branch, and goversion from which mysqld_exporter was built.
# TYPE mysqld_exporter_build_info gauge
mysqld_exporter_build_info{branch="HEAD",goversion="go1.12.7",revision="48667bf7c3b438b5e93b259f3d17b70a7c9aff96",version="0.12.1"} 1
# HELP process_cpu_seconds_total Total user and system CPU time spent in seconds.
# TYPE process_cpu_seconds_total counter
process_cpu_seconds_total 245.98
# HELP process_max_fds Maximum number of open file descriptors.
# TYPE process_max_fds gauge
process_max_fds 1024
# HELP process_open_fds Number of open file descriptors.
# TYPE process_open_fds gauge
process_open_fds 9
# HELP process_resident_memory_bytes Resident memory size in bytes.
# TYPE process_resident_memory_bytes gauge
process_resident_memory_bytes 1.5839232e+07
# HELP process_start_time_seconds Start time of the process since unix epoch in seconds.
# TYPE process_start_time_seconds gauge
process_start_time_seconds 1.60438649694e+09
# HELP process_virtual_memory_bytes Virtual memory size in bytes.
# TYPE process_virtual_memory_bytes gauge
process_virtual_memory_bytes 1.1798528e+08
# HELP process_virtual_memory_max_bytes Maximum amount of virtual memory available in bytes.
# TYPE process_virtual_memory_max_bytes gauge
process_virtual_memory_max_bytes -1
# HELP promhttp_metric_handler_requests_in_flight Current number of scrapes being served.
# TYPE promhttp_metric_handler_requests_in_flight gauge
promhttp_metric_handler_requests_in_flight 1
# HELP promhttp_metric_handler_requests_total Total number of scrapes by HTTP status code.
# TYPE promhttp_metric_handler_requests_total counter
promhttp_metric_handler_requests_total{code="200"} 4579
promhttp_metric_handler_requests_total{code="500"} 0
promhttp_metric_handler_requests_total{code="503"} 0
```

#### 常见错误： mysql_up == 0 

![image-20201104100540391](https://i.loli.net/2020/11/04/7TwoL3zCKYE4ney.png)

一般是数据库没有配置好，先检查 配置文件地址

#### prometheus监控，修改配置

**vim prometheus.yml**

```shell
scrape_configs:
  # 添加作业并命名
  - job_name: 'mysql'
    # 静态添加node
    static_configs:
    # 指定监控端
    - targets: ['192.168.210.60:9104']
```

重启prometheus

#### 查看prometheus监控

http://192.168.210.80:9090/targets 

![image-20201104101203979](https://i.loli.net/2020/11/04/8MQkglEdbJptoA5.png)

使用promsql查看

```
mysql_global_status_uptime
```

![image-20201104101332009](https://i.loli.net/2020/11/04/FUovPtKfzb5irOG.png)

![image-20201104101505819](https://i.loli.net/2020/11/04/BmTpujJ4GAFlWKI.png)

#### 使用grafana展示，导入 7362模板

![img](https://i.loli.net/2020/11/04/lQ7HmhOkPbxvcYX.png)

![img](https://i.loli.net/2020/11/04/OTgCkP4fZwXjaRo.png)

![image-20201104101757609](https://i.loli.net/2020/11/04/OUSTCxvJ78Psa2G.png)