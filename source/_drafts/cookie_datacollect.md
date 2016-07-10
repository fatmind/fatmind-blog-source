title: cookie_datacollect
tags:
---


整理思路

- 为什么会有cookie/session ？ http协议的无状态讲起
- cookie
	- 是什么 ？有那两种存储 ？
	- 在http协议层面，自身有哪些特征？domain、path、expire
- session
	- 是什么 ？
- 可以做什么 ？
	- 以登录为例。强调：是需要双方配合的。
	- 禁用cookie怎么办 ？
	- 分布式场景，怎么办 ？


- 跨域请求
	- 什么叫跨域请求 ？要能最简单的话描述出来。http://blog.csdn.net/net_lover/article/details/5172509
	- jsonp是如何解决的 ？用进化方式讲出来。http://www.cnblogs.com/dowinning/archive/2012/04/19/json-jsonp-jquery.html


- 跨域请求：当前域访问另外一个域的资源，如：img/style/script加载外部资源等。
- 同源策略：如果两个页面的协议、域名和端口是完全相同的，那么它们就是同源的。
同源策略是为了防止从一个地址加载的脚本，去获取另一个地址的内容，或访问/设置从另外一个地址加载的内容。
- jsonp：


- 带来的问题：安全 http://stc.taobao.net/swiki/sec/web-sec
	- xss
	- crsf


- 日志采集 http://confluence.taobao.ali.com:8080/pages/viewpage.action?pageId=199707375#01_wap%E6%97%A5%E5%BF%97-beaconlog
	- 采集方式一：access日志。面临什么问题 ？
	- 采集方式二：beacon方式
		- 解决方案一的什么问题
		- 演进 ？ +js
		- 为什么用一张图片 ？
	- 如何标识唯一性 ？如何串联数据 ？
	- 与效果数据的关系 ？ spm体系
	- ajax怎么办 ？
