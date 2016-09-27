---
layout: post
title: 解决glusterfs安装rpm包报错提示依赖问题
date: 2016-9-26
categories: blog
tags: [问题总结，glusterfs]
description: 解决glusterfs安装rpm包报错提示依赖问题
---

# 前言

我们在利用rpm包安装glusterfs的时候，总会因为依赖包没有安装出现报错。今天就以3.5版本的glusterfs来说明依赖问题。

# 具体操作

安装顺序如下：
<pre>
rpm -ivh  glusterfs-libs-3.5.9-1.el7.x86_64.rpm
rpm -ivh glusterfs-3.5.9-1.el7.x86_64.rpm 
rpm -ivh glusterfs-cli-3.5.9-1.el7.x86_64.rpm
rpm -ivh glusterfs-api-3.5.9-1.el7.x86_64.rpm 
rpm -ivh glusterfs-fuse-3.5.9-1.el7.x86_64.rpm
rpm -ivh glusterfs-server-3.5.9-1.el7.x86_64.rpm
rpm -ivh glusterfs-geo-replication-3.5.9-1.el7.x86_64.rpm
rpm -ivh glusterfs-extra-xlators-3.5.9-1.el7.x86_64.rpm 
rpm -ivh glusterfs-devel-3.5.9-1.el7.x86_64.rpm
rpm -ivh glusterfs-debuginfo-3.5.9-1.el7.x86_64.rpm 
rpm -ivh glusterfs-api-devel-3.5.9-1.el7.x86_64.rpm  
rpm -ivh glusterfs-rdma-3.5.9-1.el7.x86_64.rpm
</pre>

在安装最后一个rpm包的时候提示如下错误信息。
<pre>
root@linux-node1-219 src]# rpm -ivh glusterfs-rdma-3.5.9-1.el7.x86_64.rpm 
警告：glusterfs-rdma-3.5.9-1.el7.x86_64.rpm: 头V4 RSA/SHA256 Signature, 密钥 ID d5dc52dc: NOKEY
错误：依赖检测失败：
	libibverbs.so.1()(64bit) 被 glusterfs-rdma-3.5.9-1.el7.x86_64 需要
	libibverbs.so.1(IBVERBS_1.0)(64bit) 被 glusterfs-rdma-3.5.9-1.el7.x86_64 需要
	libibverbs.so.1(IBVERBS_1.1)(64bit) 被 glusterfs-rdma-3.5.9-1.el7.x86_64 需要
	librdmacm.so.1()(64bit) 被 glusterfs-rdma-3.5.9-1.el7.x86_64 需要
	librdmacm.so.1(RDMACM_1.0)(64bit) 被 glusterfs-rdma-3.5.9-1.el7.x86_64 需要

</pre>

解决办法：

我在系统优化的时候都会安装如下软件来解决依赖问题,注意rpm包安装是有顺序的。
<pre>
yum install -y rsyslog-mmjsonparse  attr nfs-utils psmisc rpcbind liburcu* userspace-rcu vim lrzsz net-tools lvm* libibverbs  librdmacm 
</pre>

that's all，enjoy！！

参考博文：[http://yunjilian.com/index.php/sec/50.html](http://yunjilian.com/index.php/sec/50.html)