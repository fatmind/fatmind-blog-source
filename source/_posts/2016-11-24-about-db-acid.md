---
title: 重新理解数据库事务ACID
date: 2016-11-24 11:36:27
---

这几个概念是大家熟知的，如何突破概念，理解其产生的缘由是下面要阐述的。
> 1.原子性：事务作为一个整体被执行，包含在其中的对数据库的操作，要么全部被执行，要么全部不执行。
> 2.一致性：事务应保证数据库的状态从一个一致状态转变为另一个一致状态。
> 3.持久性：已提交的事务对数据库的修改应该永久保存在数据库中。
> 4.隔离性：多个事务并发执行时，定义数据库系统中一个操作的结果在何时以何种方式对其它事务可见。
<!-- more -->

事务是数据库管理系统执行过程中的一个逻辑单元，是由一个有限的操作序列构成。下面让我们以具体例子来分析：
`假定：李雷要给韩梅梅转100块钱`
`李雷账户是pk=1 , 韩梅梅的账户是pk=2`
`begin transaction;`
`{查看李雷是否有一百元}`
` select cash from T where pk = 1; A操作`
`{确定有足够的钱，减少李雷的钱}`
`update T set cash = cach-100 where pk = 1; B操作`
`{给韩梅梅增加一百元}`
`update T set cach = cash+100 where pk =2; C操作`
`commit;`

###### 原子性
先来看看原子性，业务会要求：李雷账户少100块，必须在韩梅梅账户加上100块，如果中间有任何一个操作失败，系统不能凭空多100或少100。因此必须保证“李雷减钱、韩梅梅加钱”，要么同时成功，要么同时失败。这是原子性的含义。

###### 一致性
“李雷看到账户少了100，打电话给韩梅梅说转过去了，梅梅在账户上没查到”。就是一致性没得到保证而出现的一种情况。简单说，就是其他用户看到了事务操作过程中的“中间状态”，是违反用户常识的，应该被避免。
因此要保证：当李雷看到自己账户减少了100元的时候，韩梅梅的账户里也必须能够看到增加了100元。要么同时出现（转钱成功状态）、要么同时消失的保证（未转钱状态），就是事务一致性。
一致状态的含义是数据库中的数据应满足完整性约束，一方面如无符号不能出现负数，另一方面是无中间状态，如从“未转钱”到“转成功”，没有一个“转一部分”状态。

###### 原子与一致的差异性？
看起来它们都在描述：要么同时成功，要么同时失败？区分这两个概念，核心在一个“看”字。一致性约束的是，当一个用户修改记录提交后，其他用户读这条数据时，看到的“未转账状态”或“已转账状态”，而在这两个状态的中间状态不应该被看到。原子性只管“最终”，只保证操作逻辑性，不约束可见性。

###### 持久性
应该是争议的一个点，不能因为断电导致回退到之前的状态，一个很基础的保证。

###### 隔离性
先不考虑隔离性，看看ACD是否已经能保证逻辑的正确性，是没问题的。再从实现角度考虑，在多个事务并发的场景下，如何保证“多个操作组成的逻辑单元”达到ACD效果，只能用“锁”去做访问控制，模拟“减钱加钱”看似一步完成。
“锁”在自然会有性能的影响，隔离级别的不同，会受到锁影响的事务个数也不同。如：读未提交、读已提交，前者不会影响读事务，后者会影响到读事务。
而且你发现，“读未提交”的隔离级别，其实已经违反了事务一致性的要求。
与上面为了保证一次事务逻辑正确性，不同的是，隔离性主要解决的问题是性能，或者更直接点，就是尽可能的降低受到锁影响的事务进程的个数。
有没有感觉到：一致性和隔离性应该被一起考虑，一致性描述的是理想的事务过程应该怎样，而理想的事务因为锁的范围太大以至于性能难以接受，所以就使用隔离性的多个隔离级别来破坏一致性应该给出的保证，以换取更小的锁范围，从而获取更高的性能。

> 1.以上内容是阅读沈公子的文章，按自己理解梳理一遍，原始文章请点击
> [http://blog.sina.com.cn/s/blog\_693f08470101l8bf.html][1]
> 2.其它资料：
> [https://zh.wikipedia.org/wiki/%E6%95%B0%E6%8D%AE%E5%BA%93%E4%BA%8B%E5%8A%A1][2]
> [https://zh.wikipedia.org/wiki/%E4%BA%8B%E5%8B%99%E9%9A%94%E9%9B%A2][3]

[1]:	http://blog.sina.com.cn/s/blog_693f08470101l8bf.html
[2]:	https://zh.wikipedia.org/wiki/%E6%95%B0%E6%8D%AE%E5%BA%93%E4%BA%8B%E5%8A%A1
[3]:	https://zh.wikipedia.org/wiki/%E4%BA%8B%E5%8B%99%E9%9A%94%E9%9B%A2