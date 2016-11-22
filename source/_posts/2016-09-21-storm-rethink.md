---
title: storm反思
date: 2016-09-21 14:57:26
tags:
---

开头写点什么牛逼的话呢

<!-- more -->

##### 一、基础扫盲篇

Apache Storm is a free and open source distributed realtime computation system. Storm makes it easy to reliably process unbounded streams of data, doing for realtime processing what Hadoop did for batch processing. Storm is simple, can be used with any programming language, and is a lot of fun to use!

1.逻辑结构
Streams、spout、bolt、group -> topology

2.StreamId作用
Stream是一种抽象，想象下源源不断的水流，从bolt角度去看是同样道理，A应该接受那个流，StreamId就是给这个流起个名字，当构建topology时，bolt就可以声明我要处理那个流。
默认设置DEFAULT_STREAM_ID，当出现水流分拆，如：Different bolt, different tuple 例子
https://github.com/alibaba/jstorm/wiki/stream-split-merge#different-bolt-different-tuple

3.worker与executor与task
http://storm.apache.org/releases/1.0.2/Understanding-the-parallelism-of-a-Storm-topology.html
没有那篇文章比这个更清楚，五星推荐

4.物理结构
![部署结构](http://www.ituring.com.cn/figures/2015/Storm/03.d01z.001.png)
a、nimbus：任务调度节点，负责topology提交、任务分配、失败处理等
b、supervisor：实际执行任务节点
c、zookeeper：存储分配信息和supervisor节点状态

##### 二、宏观角度：当一个topology提交后生命周期
http://storm.apache.org/releases/current/Lifecycle-of-a-topology.html

1.提交
通过client脚本提交jar包，与nimbus是通过thrift通信。
a、上传jar包，存储在nimbus本地；上传成功后，再commit接口，将topology配置序列化成JSON
b、nimbus检查配置，The main purpose of normalization is to ensure that every single task will have the same serialization registrations。
c、jar与config存储在本地 {nimbus local dir}/stormdist/{topology id}

2.分配
从zk获取可用可用资源，计算，写入zk，具体参见此文章
http://www.cnblogs.com/Chuck-wu/p/4948529.html

3.执行
a、supervisor运行后台程序
This is called whenever assignments in Zookeeper change and also every 10 seconds.
- 读取分配其任务，与缓存信息做对比，若有变化则处理
- 启动或停止worker进程
- 在本地写入worker相关信息，监控状态

b、worker
- Worker connects to other workers and starts a thread to monitor for changes。
- The worker launches the actual tasks as threads within it。

##### 三、容错和监控

1.Topology监控
a、Nimbus和Supervisor之间通过/storm/supervisor/supervisor-id路径对应的数据进行心跳保持。该节点是临时节点，只要Supervisor死掉，对应路径的数据就会被删掉，Nimbus就会将原本分配给改Supervisor的任务重新分配。
b、When a worker dies, the supervisor will restart it. If it continuously fails on startup and is unable to heartbeat to Nimbus, Nimbus will reschedule the worker。
- Worker和Nimbus之间通过/storm/workerbeats/topology-id/node-port路径中的数据进行心跳维持。Nimbus会每隔一段时间获取该路径下的数据，同时Nimbus还会在它的内存中保存上一次的信息。如果发现某个Worker的心跳信息有一段时间没有更新，就认为该worker已经死掉了，Nimbus会对任务进行重新分配。
- Worker与Supervisor之间通过本地文件（LocalState）进行心跳保持。

2.后台线程
The Nimbus and Supervisor daemons are designed to be fail-fast (process self-destructs whenever any unexpected situation is encountered) and stateless (all state is kept in Zookeeper or on disk). As described in Setting up a Storm cluster, the Nimbus and Supervisor daemons must be run under supervision using a tool like daemontools or monit. So if the Nimbus or Supervisor daemons die, they restart like nothing happened.
nimbus和supervisor后台进程是设计成快速失败（出现异常则自毁）和无状态（状态维护在ZK），而且是被监控的，如果挂了会自动重启。

##### 四、数据的传递

1.通信
2.路由

##### 五、如何确保消息不丢


##### 六、有趣细节点

1.Spout怎么获取数据？
2.流控怎么做？

##### 七、在解决什么问题？适用场景？

##### 八、流式编程模型

##### 九、如何类比任务调度

##### 十、与Hadoop比较


