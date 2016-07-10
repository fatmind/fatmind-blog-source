title: 搭建zookeeper分布式环境
date: 2014-04-30
tags:
---

*记录：如何搭建zookeeper环境*

一、准备

1.Java环境，忽略  
2.下载zookeeper http://zookeeper.apache.org/releases.html （此例子中使用3.4.3.1）  
3.说明：只有一台机器，所以搭建的是一个伪分布式环境


二、动手

1.先理解下单个怎么搞 ?
http://zookeeper.apache.org/doc/trunk/zookeeperStarted.html#sc_InstallingSingleMode

2.开始

a、目录结构
```ruby
.
|-- dataStore
|-- zookeeper-3.4.3-1
|-- zookeeper-3.4.3-2
|-- zookeeper-3.4.3-3

dataStore/
|-- 1
|-- 2
|-- 3
```

b、以zookeeper-3.4.3-1为例，参见zoo.cfg
```ruby
tickTime=2000 #It is used to do heartbeats and the minimum session timeout will be twice the tickTime
dataDir=/home/bohan.sj/zk/dataStore/1 #the location to store the in-memory database snapshots
clientPort=2181 #the port to listen for client connections
initLimit=5 #接受客户端初始化连接时最长能忍受多少个心跳时间间隔数
syncLimit=2 #标识 Leader 与 Follower 之间发送消息，请求和应答时间长度，最长不能超过多少个 tickTime 的时间长度
server.1=localhost:2881:3881
server.2=localhost:2882:3882
server.3=localhost:2883:3883
#server.A = B:C:D : A表示这个是第几号服务器,B 是这个服务器的 ip 地址；C 表示的是这个服务器与集群中的 Leader 服务器交换信息的端口；D 表示的是万一集群中的 Leader 服务器挂了，需要一个端口来重新进行选举，选出一个新的 Leader
```

c、创建myid文件  
在集群模式下，需要通过myid来确定是哪一个server。在%dataDir%下新建目录，目录名即server.xxx的xxx，见dataStore目录结构

d、执行启动脚本  
依次到每个目录下执行：./zkServer.sh start

e、验证  
./zkCli.sh -server 127.0.0.1:2181（是zookeeper-3.4.3-1的clientPort）  


三、参考资料  
http://www.wangyuxiong.com/archives/51712


