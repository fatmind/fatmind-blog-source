title: "android_read_note"
tags:
---

阅读深入android相关源码，参考文章http://blog.csdn.net/luoshengyang/article/details/8923485
自己不会挖很深，主要从实际使用的角度去看


- - -
**1.logger **
程序调式除过debug，很多时候是依赖log。android提供LOG来支持。
使用角度：
-理解日志级别
-会用 http://blog.csdn.net/luoshengyang/article/details/6581828

**2.binder **
android提供的跨进程的通信方式。
在linux体系下，已有很多，见：http://blog.csdn.net/21aspnet/article/details/7479469
而android提供是叫binder概念：http://my.oschina.net/youranhongcha/blog/149578