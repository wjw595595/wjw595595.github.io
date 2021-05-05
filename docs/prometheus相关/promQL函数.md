https://yunlzheng.gitbook.io/prometheus-book/parti-prometheus-ji-chu/promql/prometheus-promql-functions

https://www.jianshu.com/p/c1eeb818a36c/

https://www.cnblogs.com/wayne-liu/p/9273492.html

# Promql函数

## rate()

rate函数，rate用来计算两个 间隔时间内发生的变化率（一段时间内平均每秒的增量）。

专门用来搭配Counters类型的数据，rate(指标名{筛选条件}[时间间隔])

比如 查看1分钟内非idle的cpu使用率

```
rate(node_cpu_seconds_total{mode!="idle"}[1m])
```

## irate()

### rate与irate的区别

irate和rate都会用于计算某个指标在一定时间间隔内的变化速率。但是它们的计算方法有所不同：irate取的是在指定时间范围内的最近两个数据点来算速率，而rate会取指定时间范围内所有数据点，算出一组速率，然后取平均值作为结果。

所以官网文档说：irate适合快速变化的计数器（counter），而rate适合缓慢变化的计数器（counter）。

根据以上算法我们也可以理解，对于快速变化的计数器，如果使用rate，因为使用了平均值，很容易把峰值削平。除非我们把时间间隔设置得足够小，就能够减弱这种效应。

https://www.cnblogs.com/orange-lsc/p/12825562.html#irate

## step对于查询结果的影响

Prometheues在对PromQL表达式求值的逻辑是这样的

1. 对于[start, end]时间区间，从start开始，以step为长度，把时间区间分成若干段
2. 对每个段进行求值

举例：`start=10,end=20,step=2`，那么就会有`ts=10,ts=12,ts=14,ts=16,ts=18,ts=20`6段，然后为这6个段进行求值。求值方式视乎表达式中Time series selector的类型而定。

