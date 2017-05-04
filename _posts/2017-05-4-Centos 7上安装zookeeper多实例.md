---
layout: post
title: Centos7上安装zookeeper 多实例
date: 2017-05-4
categories: blog
tags: [zookeeper,linux]
description: 如何在Centos7上安装zookeeper 多实例
---

## 前言

如何在Centos7上安装zookeeper 多实例
## 具体介绍

<pre>
cd /usr/local/src/
wget https://mirrors.tuna.tsinghua.edu.cn/apache/zookeeper/zookeeper-3.4.9/zookeeper-3.4.9.tar.gz
tar xvf zookeeper-3.4.9.tar.gz

ln -s /usr/local/src/zookeeper-3.4.9 /usr/local/zookeeper
 cd /usr/local/zookeeper/conf/
cp zoo_sample.cfg zoo.cfg
修改配置文件

grep "^[a-z]" zoo.cfg
	tickTime=2000
	initLimit=10
	syncLimit=5
	dataDir=/data/zk1
	dataLogDir=/data/zk1.log
	clientPort=2181
	server.1=10.10.20.204:3181:4181
	server.2=10.10.20.204:3182:4182
	server.3=10.10.20.204:3183:4183

创建3个zookeeper数据存放目录
mkdir -p /data/{zk1,zk2,zk3}
echo "1" >/data/zk1/myid
echo "2">/data/zk2/myid
 echo "3">/data/zk3/myid

生产3份配置zookeeper配置文件
cp zoo.cfg zk{1..3}.cfg
修改zk2.cfg和zk3.cfg的配置，使用对应的数据目录和端口
sed -i 's/zk1/zk2/g' zk2.cfg
 sed -i 's/2181/2182/g' zk2.cfg
sed -i 's/zk1/zk3/g' zk3.cfg
sed -i 's/2181/2183/g' zk3.cfg

启动服务
/usr/local/zookeeper/bin/zkServer.sh start /usr/local/zookeeper/conf/zk1.cfg 
usr/local/zookeeper/bin/zkServer.sh start /usr/local/zookeeper/conf/zk2.cfg 
/usr/local/zookeeper/bin/zkServer.sh start /usr/local/zookeeper/conf/zk3.cfg 
需要注意的是要有Java环境

查看状态
/usr/local/zookeeper/bin/zkServer.sh status /usr/local/zookeeper/conf/zk1.cfg
/usr/local/zookeeper/bin/zkServer.sh status /usr/local/zookeeper/conf/zk2.cfg
/usr/local/zookeeper/bin/zkServer.sh status /usr/local/zookeeper/conf/zk3.cfg

结束！

</pre>
