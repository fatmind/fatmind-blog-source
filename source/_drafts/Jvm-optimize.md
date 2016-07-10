title: Jvm_optimize
tags:
---

### 一、常见回收算法

1.直观思路
a、引用计数：
-维护计数器，每个对象被引用后，计数增加1
-面临A与B循环引用，会导致内存泄露
b、标记清除：
-从根节点追溯，标记有引用关系的对象，未被引用的则是垃圾对象；清除阶段，清理所有未被标记的对象
-面临内存碎片问题，分配效率会下降
c、复制算法：
-在标记清除再进一步，用空间换时间，预留一部分空闲区域，在垃圾回收时将正在使用的内存中的存活对象复制到未被使用的内存块中
-需要找到合适的场景
d、标记压缩
-在标记清除基础上优化，仍从根节点入手，在清理后把存活的对象，压缩到内存一端，解决碎片问题
e、其它
-为解决垃圾回收时暂停问题，延伸：增量（过程中有新垃圾数据，整体会提升回收成本）、并发（提升并发度降低耗时）
-分代：没有通用的最优算法，划分内存区域，各自采用不同的策略


2.实际算法


### 二、日志输出和格式配置


### 三、几个问题

1.reference引用，for循环例子 ？

2.native mem 是那部分 ？

3.useparallelgc 与 useparnewgc 差异 ？
http://stackoverflow.com/questions/2101518/difference-between-xxuseparallelgc-and-xxuseparnewgc

4.gc log 配置
http://itindex.net/detail/50104-jvm-%E6%97%A5%E5%BF%97-%E7%90%86%E8%A7%A3

5.XX:ReservedCodeCacheSize作用？
http://rednaxelafx.iteye.com/blog/1022095，-XX:+TieredCompilation含义
http://stackoverflow.com/questions/7513185/what-are-reservedcodecachesize-and-initialcodecachesize

6.TLAB
http://robsjava.blogspot.jp/2013/03/what-are-thread-local-allocation-buffers.html
https://docs.oracle.com/cd/E13209_01/wlcp/wlss30/configwlss/jvmgc.html


>引用：
-GC原理描述
https://www.ibm.com/developerworks/cn/java/j-lo-JVMGarbageCollection/
http://www.cubrid.org/blog/dev-platform/understanding-java-garbage-collection/
-调优
http://www.oracle.com/technetwork/java/gc1-4-2-135950.html#3.2.%20The%20Young%20Generation|outline