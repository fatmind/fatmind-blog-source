title: rpc_essential
date: 2014-08-05 15:20:16
tags:
---

伴随着‘模块化’设计流行，当完成某一个业务逻辑时，需要远程调用其它系统一起配合才能完成。
那它的基础原理是什么呢 ？


一、两台机器通信

在tcp/ip五层协议中，tcp已经能提供‘数据可靠传输’。当系统接受io请求，其处理发展可参见 [io handle evolution](http://fatmind.github.io/2014/07/14/io_handle_evolution/)，在Java世界里有很多优秀框架，如：mina、netty。

socket是被复用的，两台机器之间一般只会建立一个长连接，新的问题：如何保证请求和响应能一一对应 ？ 请求时生成reqId，在client端维护 'reqId:feature' 映射关系，当响应时返回此reqId，找到feature，唤醒被阻塞client业务线程。
>举例如下：
1.Tair
TairClient.invoke() + TairClientProcessor，每个请求有一个queue，且建立'reqId:queue'映射关系，业务现成queue.poll阻塞；等待响应queue.put唤醒业务线程
2.Hsf
生成代理类HSFServiceProxy.invoke -> RPCProtocolTemplateComponent.invoke0 -> client.futureInvoke，业务线程future.get阻塞；当响应NettyClientHandler.channelRead唤醒业务线程。

二、序列化

已是tcp传输，则必须能序列化为字节流。如何提高效？则是各种框架一直在追求的，hessain、google buffer、thrift 等等 //TODO 待补充
