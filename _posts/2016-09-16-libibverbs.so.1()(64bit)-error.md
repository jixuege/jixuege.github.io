---
layout: post
title: 解决libibverbs.so.1()(64bit) 被依赖问题
date: 2016-9-16
categories: blog
tags: [总结,错误]
description: 解决libibverbs.so.1()(64bit) 被依赖问题。
---

# 问题描述 #

 在安装glusterfs的时候出现下面报错
<pre>
#rpm -ivh glusterfs-rdma-3.5.9-1.el7.x86_64.rpm
</pre>
安装rpm 是提示如下错误：
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


# 解决办法 #
<pre>
yum -y install   libibverbs  librdmacm 
</pre>
that's all ，enjoy！！


