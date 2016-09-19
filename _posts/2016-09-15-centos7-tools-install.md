---
layout: post
title:  [已解决]CentOS 7安装后没有killall、ifconfig、命令补全
date: 2016-9-15
categories: blog
tags: [总结,错误]
description: CentOS 7安装后没有killall、ifconfig、命令补全的解决办法。
---

# 问题描述 #

 从centos6 转到centos7发现少了很多常用命令,下面就常见的几个做一个说明



# 解决办法 #

- 没有killall命令：
<pre>
yum install  -y psmisc
</pre>

- 没有ifconfig命令：
<pre>
yum install -y net-tools
</pre>
- 没有命令补全：
<pre>
 yum install -y bash-completion
</pre>

that's all ，enjoy！！


