title: seckill
tags:
---


一、业务

1. 需求
以低价的形式吸引眼球，带来流量，从而带动其它物品的销售

2. 问题
可秒杀几乎不能为普通的卖家带来过多的

胡斐总结：
1.秒杀：限制性的营销活动，限时+限量+限价
2.用超低价吸引人气，怎么卖掉其它东西。
-事前流量
-事后流量
3.秒杀不能常玩，伤元气
-对老客户造成伤害
-做秒杀劳神
-一定程度能拉动新客户，但维护客户能力有限

http://www.zhihu.com/question/19621980 淘宝秒杀活动效果
http://bbs.paidai.com/topic/79554 真正的秒杀

二、技术
秒杀讲座视频
http://xue.alibaba-inc.com/trs/searchIndex.htm?_input_charset=UTF-8&keyword=%E5%A4%A7%E5%9E%8B%E7%A7%92%E6%9D%80&category=

yy：
1.若直连数据库，存在问题，如：前端页面直连，数据库连接池压力过大；逻辑内聚，不会到处散落
2.秒杀：大流量+瞬时+低价

核心：
在各个节点都面临，大并发问题
1.detail页：
-大访问：cdn
-如何定时显示开始按钮：服务端计算推送；客户端js，与server端同步时间戳
2.交易
-下单
3.库存
-扣减库存
-超时
4.付款
-xxx

防超卖体系
http://www.atatech.org/article/detail/14076/0

三、12306
http://coolshell.cn/articles/6470.html
http://blog.codingnow.com/2012/01/ticket_queue.html


