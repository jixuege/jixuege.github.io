---
layout: post
title: Kafka及Zookeeper快速入门
date: 2016-10-09
categories: blog
tags: [软件，kafka]
description: Kafka及Zookeeper快速入门
---

# 前言

我们知道，学习一个新的东西的快速入门的四大步骤就是：安装，配置，启动，测试。
首先从kafka 快速入门之前需要学习并部署好zookeeper

# 具体操作

## Zookeeper伪集群部署

<pre>
[root@linux-node1 src]# cd /usr/local/src/
[root@linux-node1 src]# wget http://mirrors.cnnic.cn/apache/zookeeper/zookeeper-3.4.6/zookeeper-3.4.6.tar.gz
[root@linux-node1 src]# tar xf zookeeper-3.4.6.tar.gz 
# mv zookeeper-3.4.6 /usr/local/
 ln -s /usr/local/zookeeper-3.4.6/ /usr/local/zookeeper
[root@linux-node1 src]# cd /usr/local/zookeeper/conf/
[root@linux-node1 conf]# mv zoo_sample.cfg zoo.cfg
[root@linux-node1 conf]# grep '^[a-z]' zoo.cfg
tickTime=2000
initLimit=10
syncLimit=5
dataDir=/data/zk1
clientPort=2181
server.1=192.168.56.11:3181:4181
server.2=192.168.56.11:3182:4182
server.3=192.168.56.11:3183:4183
[root@linux-node1 conf]# mkdir -p /data/{zk1,zk2,zk3}
[root@linux-node1 conf]# echo "1" >/data/zk1/myid
[root@linux-node1 conf]# echo "2" >/data/zk2/myid
[root@linux-node1 conf]#  echo "3" >/data/zk3/myid
[root@linux-node1 conf]# cp zoo.cfg zk1.cfg
[root@linux-node1 conf]# cp zoo.cfg zk2.cfg
[root@linux-node1 conf]# cp zoo.cfg zk3.cfg
[root@linux-node1 conf]# sed -i 's/zk1/zk2/g' zk2.cfg
[root@linux-node1 conf]# sed -i 's/2181/2182/g' zk2.cfg
[root@linux-node1 conf]# sed -i 's/zk1/zk3/g' zk3.cfg
[root@linux-node1 conf]# sed -i 's/2181/2183/g' zk3.cfg
启动服务
[root@linux-node1 conf]# /usr/local/zookeeper/bin/zkServer.sh start /usr/local/zookeeper/conf/zk1.cfg
JMX enabled by default
Using config: /usr/local/zookeeper/conf/zk1.cfg
Starting zookeeper ... STARTED
[root@linux-node1 conf]# /usr/local/zookeeper/bin/zkServer.sh start /usr/local/zookeeper/conf/zk2.cfg
JMX enabled by default
Using config: /usr/local/zookeeper/conf/zk2.cfg
Starting zookeeper ... ^[[ASTARTED
[root@linux-node1 conf]# /usr/local/zookeeper/bin/zkServer.sh start /usr/local/zookeeper/conf/zk3.cfg
</pre>

![如图](http://7xwp9m.com1.z0.glb.clouddn.com/kafka.png_jixuege)

kafka的三大名词
>话题（topic） 是特定类型的消息六。消息是字节的有效负载（payload），话题是消息的分类名或种子（Feed）名。
生产者（producer）：是能够发布消息到话题的任何对象。已发布的消息保存在一组服务器中，他们被称为代理（Broker）或kafka集群。
消费者： 可以订阅一个或多个话题，并从Broker啦数据，从而消费这些已发布的消息。

## kafka安装

<pre>
[root@linux-node1 src]# cd /usr/local/src/
[root@linux-node1 src]# wget http://ftp.kddilabs.jp/infosystems/apache/kafka/0.9.0.1/kafka_2.11-0.9.0.1.tgz
[root@linux-node1 src]# tar zxf kafka_2.11-0.9.0.1.tgz 
[root@linux-node1 src]# mv kafka_2.11-0.9.0.1 /usr/local/
[root@linux-node1 src]# ln -s /usr/local/kafka_2.11-0.9.0.1/ /usr/local/kafka
</pre>

## kafka配置：设置zookeeper地址

<pre>
[root@linux-node1 conf]# vim /usr/local/kafka/config/server.properties
zookeeper.connect=192.168.56.11:2181,192.168.56.11:2182,192.168.56.11:2183
</pre>

## kafka启动

<pre>
[root@linux-node1 kafka]# ./bin/kafka-server-start.sh ./config/server.properties 
</pre>

## kafka测试

创建一个topic名称为test，设置一个分区和一个备份
<pre>
[root@linux-node1 ~]# cd /usr/local/kafka/bin/
[root@linux-node1 bin]# ./kafka-topics.sh --create --zookeeper 192.168.56.11:2181 --replication-factor 1 --partitions 1 --topic test
Created topic "test".

</pre>


查看已经存在的topic
<pre>
[root@linux-node1 bin]# ./kafka-topics.sh --list --zookeeper 192.168.56.11:2181
test
</pre>

发送消息
>Kafka提供了一个命令行的工具，可以从输入文件或者命令行中读取消息并发送给Kafka集群。每一行是一条消息。
<pre>
[root@linux-node1 bin]# ./kafka-console-producer.sh --broker-list 192.168.56.11:9092 --topic test
this is a message
this is another message
</pre>


消费消息
>Kafka也提供了一个消费消息的命令行工具。

<pre>
[root@linux-node1 bin]#  ./kafka-console-consumer.sh --zookeeper 192.168.56.11:2181 --topic test --from-beginning
this is a message
this is another message
hello
xixi
</pre>

 好的。只要你不Ctrl+C关闭这个经常，现在你可以尝试不停的在发送消息的终端输入内容，你就会在消费消息的终端输出内容。

thats all,enjoy!!