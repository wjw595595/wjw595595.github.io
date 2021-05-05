参考：

https://www.cnblogs.com/guoxiangyue/p/11778217.html

# 部署 snmp_exporter

说明：snmp_exporter的配置文件需要自己通过SNMP Exporter Config Generator 项目编译生成

说明：由于Prometheus使用go语言开发的，所以自己编译生成snmp_exporter的配置文件需要go环境

## 下载snmp_exporter

下载snmp_exporter安装包，下载地址：https://github.com/prometheus/snmp_exporter/releases 

下载完成后，上传至机器的 /usr/local 目录下

 解压安装包

```
#   tar -zvxf snmp_exporter-0.15.0.linux-arm64.tar.gz -C /usr/local
#   mv snmp_exporter-0.15.0.linux-arm64/ snmp_exporter
```

配置snmp_exporter



## 安装go环境

https://golang.org/dl/ 

安装包下载以后，上传至监控主机的 /usr/local 目录下

```
tar -xvf go1.13.1.linux-amd64.tar.gz
```

配置环境变量

添加/usr/loacl/go/bin目录到PATH变量中。添加到/etc/profile 或$HOME/.profile都可以

```
# vim /etc/profile
// 在最后一行添加
export GOROOT=/usr/local/go
export PATH=$PATH:$GOROOT/bin
// wq保存退出后source一下
# source /etc/profile
```

执行go version，如果显示版本号，则Go环境安装成功。

## 构建：snmp exporter config Generator

先解决go get慢的问题

建议go版本在1.13以上

```
##linux
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
go env #查看配置

windows
powershell
PS C:\Users\mark> $env:GOPROXY = "https://goproxy.cn"

修改环境变量；
echo "export GOPROXY=https://goproxy.cn" >> ~/.profile && source ~/.profile

#go下载使用mod模式,下载会在 GOPATH的pkg/mod 目录下
```



```
#  yum -y install git
#  yum -y install gcc gcc-g++ make net-snmp net-snmp-utils net-snmp-libs net-snmp-devel
#  go get github.com/prometheus/snmp_exporter/generator@v0.19.0
#  cd $GOPATH/pkg/mod/github.com/prometheus/snmp_exporter\@v0.19.0/generator
#  go build
#  make mibs  #这个可以不做
```

## 制作generator.yml文件

### 下载相应的mib库

到  https://github.com/librenms/librenms/tree/master/mibs 下载相应的mib库

[http://oidref.com](http://oidref.com/)

放在  **/root/.snmp/mibs**  里面(generator 默认到 这个目录找)

```
# unzip -d /root/.snmp/mibs MIB-V200R010C00SPC600.zip
```

### **制作generator.yml配置文件**

```shell
#cd $GOPATH/pkg/mod/github.com/prometheus/snmp_exporter\@v0.19.0/generator
# vim generator.yml 
modules:
  # Default IF-MIB interfaces table with ifIndex.
  huawei:
    walk:  
      - 1.3.6.1.2.1.2
      - sysUpTime                  # Same as "1.3.6.1.2.1.1.3"
      - sysDescr                   # Same as "1.3.6.1.2.1.1.1"
      - sysName                    # Same as "1.3.6.1.2.1.1.5" 
      - 1.3.6.1.2.1.31    
      - 1.3.6.1.4.1.2011.5.25.31.1.1
    version: 2
    auth:
      community: default
    max_repetitions: 25  # How many objects to request with GET/GETBULK, defaults to 25.
                         # May need to be reduced for buggy devices.
    retries: 3   # How many times to retry a failed request, defaults to 3.
    timeout: 5s  # Timeout for each individual SNMP request, defaults to 5s.
    lookups:
      - source_indexes: [ifIndex]
        lookup: ifAlias
      - source_indexes: [ifIndex]
        # Uis OID to avoid conflict with PaloAlto PAN-COMMON-MIB.
        lookup: 1.3.6.1.2.1.2.2.1.2 # ifDescr
      - source_indexes: [ifIndex]
        # Use OID to avoid conflict with Netscaler NS-ROOT-MIB.
        lookup: 1.3.6.1.2.1.31.1.1.1.1 # ifName
    overrides:
      ifAlias:
        ignore: true # Lookup metric
      ifDescr:
        ignore: true # Lookup metric
      ifName:
        ignore: true # Lookup metric
      ifType:
        type: EnumAsInfo
```

### 生成snmp.yml

```
#可以不export,默认到 /root/.snmp/mibs找
export MIBDIRS=/usr/share/snmp/mibs
./generator generate
#在当前目录生成snmp.yml文件
```

把snmp.yml复制到  /usr/local/snmp_exporter目录

```shell
cp snmp.yml /usr/local/snmp_exporter/
#./snmp_exporter
Ctrl+C 结束掉 snmp_exporter 进程
参考：
后台运行：nohup ./snmp_exporter --config.file=snmp.yml --web.listen-address=:9116 > default.log 2>&1 &
```

![](https://i.loli.net/2020/12/30/bgfW9k1e5lRY6vu.png)

![image-20201230183109027](https://i.loli.net/2020/12/30/4XmJfW9IBYOw5qn.png)

## snmp_exporter设置开机启动

```shell
# vim /etc/systemd/system/snmp_exporter.service
[Unit]
Description=snmp_exporter
After=network.target
Documentation=https://github.com/prometheus/snmp_exporter


[Service]
ExecStart=/usr/local/snmp_exporter/snmp_exporter \
          --config.file=/usr/local/snmp_exporter/snmp.yml \
          --web.listen-address=:9116 \
          --snmp.wrap-large-counters \      
          --log.level=info


[Install]
WantedBy=multi-user.target
```

```shell
# systemctl daemon-reload
# systemctl enable snmp_exporter
# systemctl start snmp_exporter
# systemctl status snmp_exporter
```

查看

```
# curl 'http://192.168.210.80:9116/snmp?module=huawei&target=192.168.210.254'
```

## 配置prometheus

```shell
  - job_name: 'snmp_exporter'
    scrape_interval: 1m
    scrape_timeout: 1m
    static_configs:
      - targets:
        - 192.168.210.254
    metrics_path: /snmp
    params:
      module: [huawei]
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: 192.168.210.80:9116
```

```shell
#重启prometheus
docker-compose -f docker-compose-monitor.yml restart prometheus
```

![image-20201230185042834](https://i.loli.net/2020/12/30/39eaBDsdK6MuG18.png)







## 其他参考模板

```yml
modules:
  # Default IF-MIB interfaces table with ifIndex.
  huawei:
    walk:
      - interfaces
      - sysUpTime                  # Same as "1.3.6.1.2.1.1.3"
      - ifXTable
      - sysDescr                   # Same as "1.3.6.1.2.1.1.1"
      - sysName                    # Same as "1.3.6.1.2.1.1.5" 
      - 1.3.6.1.4.1.2011.5.25.31.1.1
      #cpu
      - 1.3.6.1.4.1.25506.2.6.1.1.1.1.6.slot
      #内存
      - 1.3.6.1.4.1.25506.2.6.1.1.1.1.8.slot
    version: 2
    auth:
      community: public
    max_repetitions: 25  # How many objects to request with GET/GETBULK, defaults to 25.
                         # May need to be reduced for buggy devices.
    retries: 3   # How many times to retry a failed request, defaults to 3.
    timeout: 5s  # Timeout for each individual SNMP request, defaults to 5s.
    lookups:
      - source_indexes: [ifIndex]
        lookup: ifAlias
      - source_indexes: [ifIndex]
        # Uis OID to avoid conflict with PaloAlto PAN-COMMON-MIB.
        lookup: 1.3.6.1.2.1.2.2.1.2 # ifDescr
      - source_indexes: [ifIndex]
        # Use OID to avoid conflict with Netscaler NS-ROOT-MIB.
        lookup: 1.3.6.1.2.1.31.1.1.1.1 # ifName
    overrides:
      ifAlias:
        ignore: true # Lookup metric
      ifDescr:
        ignore: true # Lookup metric
      ifName:
        ignore: true # Lookup metric
      ifType:
        type: EnumAsInfo

```

```yml
modules:
  # Default IF-MIB interfaces table with ifIndex.  要有对应的mib库，不然不能生成 snmp.yml  ,snmp.yml不能修改，智能通过generator 生成
  huawei_mib:
    walk: 
      - sysUpTime
      - interfaces
      - ifXTable
      - sysDescr
      - sysName
      - 1.3.6.1.2.1.31.1.1.1.1
      - 1.3.6.1.4.1.2011.5.25.38
      - 1.3.6.1.2.1.80
      - 1.3.6.1.4.1.2011.5.25.31.1.1.1.1.2 #实体操作状态
      - 1.3.6.1.4.1.2011.5.25.31.1.1.1.1.5 #实体CPU使用率
      - 1.3.6.1.4.1.2011.5.25.31.1.1.1.1.7 #实体内存使用率
      - 1.3.6.1.4.1.2011.5.25.31.1.1.1.1.10 #实体启动时间
      - 1.3.6.1.4.1.2011.5.25.31.1.1.1.1.11 #实体温度
      - 1.3.6.1.2.1.80.1.2.1.4 #测试的目的地址
    version: 
    auth:
      community: public
    lookups:
    #lookups 是prometheus 一个lookup 一个标签 下面是放三个标签 ,以ifindex唯一值
      - source_indexes: [ifIndex]
        lookup: ifAlias
      - source_indexes: [ifIndex]
        # Uis OID to avoid conflict with PaloAlto PAN-COMMON-MIB.
        lookup: 1.3.6.1.2.1.2.2.1.2 # ifDescr
      - source_indexes: [ifIndex]
        # Use OID to avoid conflict with Netscaler NS-ROOT-MIB.
        lookup: 1.3.6.1.2.1.31.1.1.1.1 # ifName
    overrides:
      ifAlias:
        ignore: true # Lookup metric
      ifDescr:
        ignore: true # Lookup metric
      ifName:
        ignore: true # Lookup metric
      ifType:
        type: EnumAsInfo
```

云地址上  1.116.208.30 WJW......

```
cd /opt/go/work/pkg/mod/github.com/prometheus/snmp_exporter@v0.19.0/generator
./generator generate
#生成snmp.yml 文件
```

![image-20210427013429792](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210427013429792.png)