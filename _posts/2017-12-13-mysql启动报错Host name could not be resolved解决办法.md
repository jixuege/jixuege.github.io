---
layout: post
title: mysql 启动报错Host name could not be resolved解决办法
date: 2018-2-27
categories: blog
tags: [Linux]
description: mysql 启动报错Host name could not be resolved解决办法
---


## 前言
mysql 启动报错信息如下：
<pre>
[root@xxx ~]# 2018-01-26 17:06:35 33 [Warning] Host name 'bogon' could not be resolved: Name or service not known
2018-01-26 17:06:36 33 [Warning] Host name 'bogon' could not be resolved: Name or service not known
2018-01-26 17:06:41 33 [Warning] Host name 'bogon' could not be resolved: Name or service not known
skip-name-resolve
</pre>

## 具体介绍及操作

解决办法如下，在配置文件中的mysqld里面加入下面一条：

<pre>
[mysqld]下面一行加入skip-name-resolve重启mysql服务就可以了。

</pre>

that's all！！
