title: hbase rowkey design
date: 2014-05-12
tags:
---

题记：结合官方 [rowkey design](http://hbase.apache.org/book/rowkey.design.html)，来整理下自己的理解

1.Monotonically Increasing Row Keys/Timeseries Data  
- 问题：hbase数据是以rowkey的字典序顺序存储，当单个region数据过大时，会自动进行分裂。单调递增的rowkey，如以‘时间戳’为prefix，导致数据写入总是会落在单个region上。吞吐量（单个server）和rt（client无法并行）被限制。  
- 解决：一般生成随机数，或hash后取模，去保证rowkey能够分散开  
- 扩展：现实场景，就是有许多要基于时间序来查询的数据，如：运营操作日志查询某个时间段、微博广场按时间倒序展现。一般有两种方案：  
- 索引表，比如微博广场，则索引表=时间戳_userId_weiboId。此时尽管也会面临落在单个region问题，但数据量小，可接受。但hbase是无事务的，因此数据一致性要求高时，还需要其它方案来保证。  
- opentsdb是用于监控系统，其数据量自然很庞大。其rowkey定义 [metric_type]_[event_timestamp]_xxx，利用metric_type去散列数据的分布，但有其局限性。

2.Try to minimize row  
- 数据需要被检索，则必须要有索引结构。当定位到某个region后（一般只会定义单个columnFamily），因此每个region也就单个Store，由1个memStore和多个storeFile组成，storeFile的dataBlock索引结构，是此block的第一个rowkey。读取此storeFile的整个索引到内存，找到对应rowkey所在block起始位置，读取整个block，遍历找到rowkey。  
- 随带提一下，实际在datablock中，存储的是KeyValue的array数组。因此首先理解KeyValue组成，则能更好的理解 ‘cf的name短一点’ 会更好，参照 http://hbase.apache.org/book/regions.arch.html#keyvalue

3.Reverse Timestamps  
A common problem in database processing is quickly finding the most recent version of a value，rowkey = [key][Long.MAX_VALUE - timestamp]。但没搞清楚，version不是已经解决此问题了吗 ？  

4.Immutability of Rowkeys  
Rowkeys cannot be changed.

5.Relationship Between RowKeys and Region Splits  
默认开始只有一个region，后续当region数据量达到阈值时，region server 会自动进行 region split。另外也可以在创建表时，预分配region，从而仍region散列在多台server，提升读写能力。但是必须要让rowkey足够散列，否则仍然会落在少数几台region server。



**参考资料:**  
1.region定位过程【good】  
http://greatwqs.iteye.com/blog/1838904  
2.region split【good】  
http://flyingdutchman.iteye.com/blog/1846141  
3.hbase tunning  
http://www.searchtb.com/2012/08/hbase-performance-tunning-in-taobao.html


