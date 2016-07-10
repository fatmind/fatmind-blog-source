title: 关于数据库join查询
date: 2014-11-30 05:20:11
tags:
---

**一、LEFT-JOIN引发的思考**

**1. 一个left-join慢查询**

>SELECT count(*) from adzones;   -- 8496380
SELECT count(*) from sites;     -- 738055
-- 在adzones表有索引
KEY `index_sites` (`siteid`,`status`,`createtime`),
-- sites表主键字段是siteid
PRIMARY KEY (`siteid`)
-- 执行left-join查询非常慢
SELECT * from adzones a LEFT JOIN sites s ON a.siteid = s.siteid;
-- 执行join则好很多
SELECT * from adzones a JOIN sites s ON a.siteid = s.siteid;

EXPLAIN结果如下，上面left-join，下面join。清楚看到其驱动表扫描的数量差一个级别。

![left-join](http://git-blog-img.qiniudn.com/mysql_left_join_explain.jpg)
![join](http://git-blog-img.qiniudn.com/mysql_join_explain.jpg)

**2. join有哪些方式**

http://www.sitepoint.com/understanding-sql-joins-mysql-database/
摘自上面这篇文章，用图形很容易表示清楚
- join（inner-join）
SELECT user.name, course.name FROM `user` JOIN `course` on user.course = course.id;
![](http://blogs.sitepointstatic.com/images/tech/512-sql-joins-inner.png)
- left-join
SELECT user.name, course.name FROM `user` LEFT JOIN `course` on user.course = course.id;
![](http://blogs.sitepointstatic.com/images/tech/512-sql-joins-left.png)
- rigth-join
SELECT user.name, course.name FROM `user` RIGHT JOIN `course` on user.course = course.id;
![](http://blogs.sitepointstatic.com/images/tech/512-sql-joins-right.png)
- outer-join（full-join）
不常用，mysql自身不支持，但可以用union方式变通
SELECT user.name, course.name FROM `user` LEFT JOIN `course` on user.course = course.id
UNION
SELECT user.name, course.name FROM `user` RIGHT JOIN `course` on user.course = course.id;
![](http://blogs.sitepointstatic.com/images/tech/512-sql-joins-outer.png)

**3.join实现方式**

参考资料 http://blog.csdn.net/panzhaobo775/article/details/7327896
- nested loop
扫描一个表，每读到一条记录，就根据关联条件去另一个表匹配。这也是mysql对join实现方式。
适合：两张表大小差异大，以小表为驱动表，且关联条件能走索引。
>回到开头，为什么left-join比join慢，因left-join明确要使用adzones作为驱动表，adzones比sites记录多一个数量级。join查询时，mysql分析后采用小表作为驱动表，所以快。
- sort merge join
先扫描所有表，进行排序，再同时遍历merge数据。很明显sort会很耗费cpu、且受制于内存。oracle支持。
适合：两个不大的表之间join，且连接条件无索引。改进版即hash-join。
http://blog.itpub.net/17203031/viewspace-697012/
- hash join
适合：必须以大表为驱动表，且连接条件无索引（或认为hash是更快的检索方式），如left-join。首先扫描小表，在内存中基于连接列建立hash表，然后再遍历大表，每读到一条就查询hash表。
必然会受限于数据量和内存的大小。oracle支持。
http://blog.itpub.net/17203031/viewspace-697442/

**4.where与on差异**

http://www.oschina.net/question/89964_65912
首先得明确select执行顺序：FROM -> WHERE -> SELECT -> GROUP BY -> HAVING -> ORDER BY
```java
SELECT * FROM product;
+----+--------+
| id | amount |
+----+--------+
|  1 |    100 |
|  2 |    200 |
|  3 |    300 |
|  4 |    400 |
+----+--------+
SELECT * FROM product_details;
+----+--------+-------+
| id | weight | exist |
+----+--------+-------+
|  2 |     22 |     0 |
|  4 |     44 |     1 |
|  5 |     55 |     0 |
|  6 |     66 |     1 |
+----+--------+-------+
-- join/on属于from部分，where是在join后结果上where去筛选结果。可以脑补下差异：
SELECT * FROM product LEFT JOIN product_details ON (product.id = product_details.id) AND   product_details.id=2;
SELECT * FROM product LEFT JOIN product_details ON (product.id = product_details.id) WHERE product_details.id=2;
-- on条件，若left-join则约束rightTable，若join则两张表都约束。请脑补：
SELECT * FROM product LEFT JOIN product_details ON product.id = product_details.id AND product.amount=100;
```

**二、分布式join**

原本在单机执行的sql，在数据库水平扩展后，两个逻辑表都进行分库分表，join怎么实现呢？要能实现join，首先得拿到数据吧，再合并。
所以如何尽量避免传输大量的数据到上层，再进行聚合/计算等，代价太大。

**优化的关键词：下推**。尽可能让其在基础数据库执行。
一种思路：先扫描左表，累积一定数量后，对右表构造in查询，下推右表。限制是连接列在右表有索引。

阿里Tddl在这方面有非常深厚的积累，暂时未开源。
大家可以多关注沈公子，都是精品 http://weibo.com/whisperxd

*后记：苍天呀，markdown格式在编辑器中和hexo编译后效果不一样，哭...*