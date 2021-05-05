文档
https://github.com/wjw595595/prometheus-book/blob/master/quickstart/why-monitor.md

# prometheus基础知识

## 概念

prometheus是一个开源的服务监控系统和事件序列数据库

监控：监控服务器、数据库、应用程序

- 官方网站：https://prometheus.io
- 项目托管：https://github.com/prometheus
https://prometheus.io/docs/instrumenting/exporters/
## prometheus server

prometheus的核心，负责实现对监控数据的获取、存储以及查询

## exporter

将监控的数据采集起来，通过http服务的形式暴露出去，prometheus server 会定时去拉取这些暴露的数据进行监控

如：mysql_exporter,snmp_exporter

监控应用程序：可以在程序中集成官方的sdk，自己埋点暴露出需要监控的指标

## PushGateway

是一个独立的服务，大部分场景用pull模式，（prometheus server拉取exporter提供的数据），如何想使用push模式，可以将数据push给 PushGateway，由pushgateway统一提供给 Server

## service Discovery

服务发现功能，在微服务场景监控很重要，服务发现可以动态的去发现服务信息，无需手动修改配置，官方推荐用Consul作为服务发现， Nacos目前不支持，（可以定时拉取信息同步到文件中）

## 三剑客

prometheus ，grafana，alertManager

alertManager负责异常告警，可以对接钉钉等多种信息通知

![img](https://i.loli.net/2020/12/24/UbucxyFzrejDTIC.png)

alertManager架构图

![img](https://i.loli.net/2020/12/24/2BwOykaUVM7rcj9.png)

1. 从左上开始，Prometheus 发送的警报到 Alertmanager;
2. 警报会被存储到 AlertProvider 中，Alertmanager 的内置实现就是包了一个 map，也就是存放在本机内存中，这里可以很容易地扩展其它 Provider;
3. Dispatcher 是一个单独的 goroutine，它会不断到 AlertProvider 拉新的警报，并且根据 YAML 配置的 Routing Tree 将警报路由到一个分组中;
4. 分组会定时进行 flush (间隔为配置参数中的 group_interval), flush 后这组警报会走一个 Notification Pipeline 链式处理;
5. Notification Pipeline 为这组警报确定发送目标，并执行抑制逻辑，静默逻辑，去重逻辑，发送与重试逻辑，实现警报的最终投递;

## promQL

查询结果有三种类型：
1、瞬时向量(Instant vector)：http_requests_total
2、区间向量(Range vector)：http_requests_total[5m]
3、标量(Scalar)（没有时间序列）：count(http_requests_total)

指标(Metric)
<metric name>{<label name>=<label value>, ...}


Prometheus定义了4种不同的指标类型
(metric type)：Counter（计数器）、Gauge（仪表盘）、Histogram（直方图）、Summary（摘要）



在time-series中的每一个点称为一个样本（sample），样本由以下三部分组成：
指标(metric)：metric name和描述当前样本特征的labelsets;
时间戳(timestamp)：一个精确到毫秒的时间戳;
样本值(value)： 一个float64的浮点型数据表示当前样本的值。



https://fuckcloudnative.io/prometheus/

https://www.cnblogs.com/lhrbest/p/14355265.html



