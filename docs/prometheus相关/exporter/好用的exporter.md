http://bubuko.com/infodetail-3524268.html

# prometheus 一些不错的exporter

## blackbox_exporter

支持协议：http、dns、tcp、icmp

ICMP：监控主机存活状态 module: [icmp]

TCP:监控主机端口存活状态module: [tcp_connect]

HTTP: module: [http_2xx]监控网站状态



## statping

监控网站应用的（状态查看的），同时暴露了prometheus metrics 参考网站：https://github.com/statping/statping

## sql_exporter

灵活的sql exporter 参考网站：https://github.com/justwatchcom/sql_exporter

## exporter_exporter

让exporter 支持类似nginx 的proxy 模式，参考网站：https://github.com/QubitProducts/exporter_exporter

## query-exporter

基于sql 暴露metrics 的类似sql_exporter,参考网站：https://github.com/albertodonato/query-exporter

## script_exporter

基于scripit 暴露metrics，参考网站：https://github.com/ricoberger/script_exporter

## systemd_exporter

提供系统systemd的一些metrics，参考网站：https://github.com/povilasv/systemd_exporter

## blackbox_exporter

一个比较通用的exporter，prometheus 官方出品，参考网站：https://github.com/prometheus/blackbox_exporter

## json-exporter

基于jsonpath+json api 模式的metrics 暴露，参考网站https://github.com/tolleiv/json-exporter

## process-exporter

系统进程信息的metrics，参考网站：https://github.com/ncabatoff/process-exporter

## ebpf_exporter

暴露ebpf 的一些metrics，参考网站：https://github.com/cloudflare/ebpf_exporter

## ssh_exporter

基于ssh 执行远程命令暴露metrics，参考网站：https://github.com/Nordstrom/ssh_exporter

## 其他的一些应用积以及db&&中间间的exporter

这类的比较多，各种db的以及redis，memecache，kafka，rabbitmq。。。。。很多