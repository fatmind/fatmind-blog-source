title: binlog与redolog纠结
date: 2016-02-16 20:36:44
tags:
---

>在很长一段时间内，一直分不清楚binlog与redolog、undolog有什么差异。春节读过[MySql技术内幕]，先来梳理下binlog与redolog。
下面计划：redolog详解、redolog与undolog、mvcc实现原理。

#####一、结论

先上结论，可以从以下几个点来辨识其差异：

**1.维度**
了解过mysql体系结构都知道，其存储引擎是插件式的。mysql存储引擎的架构提供了一系列标准的管理和服务支持，这些标准与引擎本身无关，如sql分析和优化器，而存储引擎是底层物理结构的实现，每个存储引擎开发者都可以按照自己的意愿来开发。可参见此图 http://images.cnblogs.com/cnblogs_com/yjf512/105018221.png。
binlog是mysql的，与底层的存储引擎无关；redolog仅是innodb引擎专有。

**2.内容**
binlog记录的是逻辑sql，如：update table_1 set a = 'xxx' where id = 1，内容和格式有几种可选项，如row格式；redolog记录的是关于每个页的更改物理情况。

**3.写入时间**
binlog仅在事务提交前进行提交，即只写磁盘一次，不论这事务有多大；redolog在事务进行的过程中，会有不断的重做日志条目被写到日志文件中。


#####二、

1.binlog
-作用：审计、主从复制等
-问题：
若不同步写，可能会存在数据复制丢失；若同步写，若binlog已写入，redolog未写入，此时当机，会导致数据不一致，设置support_xa=1解决此。
备库的复制执行sql应该不写入binlog。

2.redolog
-作用：cpu与磁盘在速度上的巨大差异，需要有机制能去协调这种不匹配。缓冲机制，但此时面临当机，数据丢失问题。演进的解决方案是，先写redolog，若当机，重新启动后也可通过redolog来恢复。
-思考：要保证acid的d，则redolog必须要同步写，此时仍然存在cpu与磁盘不匹配的问题？直接写表数据文件是随机的磁盘访问，写速度很慢，redolog可以帮助转换为顺序写。
-问题：
若缓存足够大，redolog文件足够大，是否没必要去同步脏页到磁盘？条件一很难满足，条件二从运维角度也面临困难，且若一旦当机恢复时间会很长。
那脏页必须有一种机制，可以刷回磁盘？checkpoint逻辑，从三个角度：缓存池不够用、redolog不够用（背后是因为redolog在被重复循环使用）。

