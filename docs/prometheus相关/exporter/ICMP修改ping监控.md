https://www.cnblogs.com/moniter/p/12305180.html

用的 blackbox_exporter 

ping 

https://github.com/knsd/ping-exporter

https://blog.csdn.net/apple_llb/article/details/50494787

https://www.cnblogs.com/xiao987334176/p/12022482.html

https://github.com/prometheus/blackbox_exporter/blob/master/example.yml

https://github.com/prometheus/blackbox_exporter/blob/master/CONFIGURATION.md

https://blog.frognew.com/2018/02/prometheus-blackbox-exporter.html



```shell
cd /usr/local/bin/blackbox_exporter
./blackbox_exporter --config.file=blackbox.yml &

cd /opt/config
docker-compose -f docker-compose-monitor.yml restart prometheus

```

https://www.iamle.com/archives/2130.html

```shell
  - job_name: 'ping_all'
    scrape_interval: 20s
    metrics_path: /probe
    params:
      module: [icmp] #ping
    static_configs:
      - targets: ['192.168.210.50','192.168.210.60','192.168.210.101']
        labels:
          group: '北京'
      - targets: 
        - 192.168.210.70
        - 192.168.210.100
        labels:
          group: '上海'
    relabel_configs:
      - source_labels: [__address__]
        regex: (.*)(:80)?
        target_label: __param_target
        replacement: ${1}
      - source_labels: [__param_target]
        regex: (.*)
        target_label: ping
        replacement: ${1}
      - source_labels: []
        regex: .*
        target_label: __address__
        replacement: 192.168.210.80:9115
```

