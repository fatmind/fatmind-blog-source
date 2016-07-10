title: connect pool theory
date: 2014-05-11
tags:
---

一. 若自己去实现connectPool会怎么做 ?  

1. 本质：中间做一个代理类，connect新建或close都由代理类处理，从而保证connect可复用  
2. 基于上面的假设，核心的点：  
\- connect容器，即管理类  
\- 原生conn包装类，以及create/close  
\- 管理：idle-conn释放、长时间conn占用释放、idle-conn有效性校验  
\- 扩展点，如：慢查询统计、防注入分析等

二. tomcat jdbc 怎么做的 ？  

1.类结构图  
![](http://git-blog-img.qiniudn.com/tomcat_pool_class.jpg)

a、DataSource：保持使用的习惯，conn从datasource来  
b、PoolConfiguration：配置类，如：MaxActive。以方法形式表示  
c、ConnectionPool：conn管理类  
d、PooledConnection：under conn 包装类  
e、PoolCleaner： pool清理类，是ConnectionPool的内部类  
f、JdbcInterceptor：拦截器，可自定义扩展，如：conn.close

2.说这么多，以getConnection/close为例，来看整体流程：  

2.1 getConnection  
>  
1.若pool为null，初始化connPool  
1.1. 配置检查，如：minIdle is larger than maxActive  
1.2. 启动PoolCleaner，做三件事情：checkAbandoned、checkIdle、testAllIdle  
1.3. borrowConnection，返回conn  
1.3.1. idle若有空闲，直接返回  
1.3.2. size是否大于maxActive  
1.3.2.1 否，创建conn，且加入busy队列（函数内部，利用lock去防止什么呢？）  
1.3.2.2 是，等待idle.poll(timetowait)时间，若仍无可用conn，抛出异常  
1.3.3 利用returnConnection函数将新conn放回到idle队列  
2.从pool获取conn，实际调用borrowConnection  
3.生成conn的interceptor，返回

2.2 从上面getConnection文字表达效果很差，自己读起来也很啰嗦。懒的再去画调用关系图。整体代码很简单，还是直接看代码更清楚。

2.3 记录下其中一些核心点如何实现 ？  

a、如何维护conn ？  
定义busy、idel两个队列，在getConn放入busy队列，close时从busy移除加入到idle；用AtomicInteger维护conn个数  

b、如何清理idel、长时间占用conn、conn有效性 ？  
都在PoolCleaner完成，继承TimerTask，线程while(true)间隔去执行  
若idel.size>minIdleSize，释放部分在idel中的conn；  
若busy中conn占用时间超过AbandonTimeout，释放此conn；  
测试当前所有idel有效性，通过执行 'SELECT 1' 能否执行成功去判断，若无效，则释放conn  

c、拦截器的作用 ？  
每次conn从
\- 生成对PooledConnection的代理类，保证close调用是将conn放回pool中
\- 自定义扩展点，如：慢查询统计等

三、资料  
1.Java reference  
http://fatmind.iteye.com/admin/blogs/1900522  
2.读写锁  
http://ifeve.com/read-write-locks/  
3.durid 阿里数据库连接池  
https://github.com/alibaba/druid







