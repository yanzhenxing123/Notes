## 背景

多贝数据部门中间表项目监控 

1. Kafka的队列累
2. 中间表的更新数量

## StatsD

StatsD 狭义上来讲，其实就是一个监听 UDP(Default)/TCP 的守护程序

基本分为了三步，

1. 根据简单的协议收集 StatsD 客户端发送来的数据;
2. 解析数据并进行聚合；
3. 定时推送给后端，如 Graphite 和 InfluxDB 等，并通过 Grafana 等展示



github链接: https://github.com/statsd/statsd/blob/master/docs/metric_types.md

## StatsD metrics

格式：`<metric_name/bucket>:<value>|<type>[|@sample_rate]`

**指标类型**： counter、timer、set、guage

### counter

```
countor:1|c
```

### timing

计时器，可以计算平均值、最大值、最小值等

```
timer:
```



### gauge

标量是任何可测量的一维变量，如内存、人的身高、体重等。

如果该值在下次刷新前没有更新，那么就会发送上次的值。也可以在值前添加正负号，用于表示对值的修改，而非设置。这也就意味着，不能直接设置负值，需要先设置为 0 才可以。

```
gaugor:333|g
gaugor:-10|g
gaugor:+4|g
```

### set

记录flush期间不重复的值。

可以针对某一个集合进行统计，统计总共出现了多少种类的值。

```
seter:765|s
```

## Python调用

Python中的.就相当于隔开了一个tag

Tag1: env

Tag2: log_type

Tag3: class_name

```python
from statsd import StatsClient

stat = f"{os.getenv('ENV', 'local')}.{log_type}.{c.__class__.__name__}"
# 中间表计算耗时
SDC_TIMING.timing(stat, int((time.time() - t) * 1000))
# 中间表计算量
SDC_TIMING.incr(stat)
# 中间表更新数量
SDC_SET.incr(stat, result.modified_count)
```