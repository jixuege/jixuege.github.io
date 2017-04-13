---
layout: post
title: NFS服务挂了，NFSclient机器上执行df命令卡死解决办法
date: 2017-04-13
categories: blog
tags: [NFS,Linux]
description:  NFS服务挂了，NFSclient机器上执行df命令卡死解决办法
---


## 前言

只要一运行df命令，该session马上卡死，使用Ctrl+C 也不能终止掉。

## 具体介绍及操作
一般就是NFS服务端服务挂了导致。
###Configuring Harbor with HTTPS Access
<pre>
办法1：
使用root用户登录，修改/etc/mtab，把关于NFS的整行删除，保存即可。

方法2：
等待NFS server恢复，NFS重启后使用，但是需要把挂载重新挂载一次。
</pre>

更新完毕！！enjoy！！

