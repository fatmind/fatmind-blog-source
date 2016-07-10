title: mq-vs-storm[疑惑篇]
date: 2014-07-15 19:33:08
tags:
---

题记：这个问题困扰我很长时间，今天总算想清楚点眉目，写下来加强理解

**1.问题**

mq（kafka）和storm有什么区别，各自合适用在什么场景 ？ 能不能说mq可以替换storm ？ 开始自己的答案是肯定的，mq可以替代storm。理由如下：

| 		        | mq           | storm  |
| ------------- |:-------------:| -----:|
| 持久化      | 持久 | 不持久 |
| 容错      | zk  | zk |
| 负载均衡 | 分区粒度散列，基于某个业务字段 | stream粒度散列，也是基于业务字段 |
| 调式难度      | 相比简单  | 复杂 |

......等等

**2.怎么办**

1.发现有很多同学也在困扰这个问题，一通google搜索，还是没能解决自己的困惑
http://www.quora.com/What-are-the-main-differences-between-Storm-and-Kafka
http://www.ymc.ch/en/storm-or-apache-kafka
http://www.zhihu.com/question/20028515
http://stackoverflow.com/questions/21808529/apache-kafka-vs-apache-storm

2.静心掏出本，画了画它们在系统结构中的位置，终于理解，总结：
- 首先一定要清楚：mq是消息中间件、storm是实时计算系统，它们处于整个系统结构的不同位置
- mq常用于2点：A -> B 系统间解耦；一人生产多人消费
- strom则完全可以是你的B系统；或者B系统是你自己写的代码。至于B系统为什么要用storm，则是另外话题

3.附storm别人思考
http://wbj0110.iteye.com/blog/1955426




