---
layout: post
title: Centos7 调整inode数量
date: 2016-10-17
categories: blog
tags: [inodce，Linux]
description: Centos7 调整inode数量
---

# 前言

容器内的inode满了，写入不了数据，下面就是如何去调整inode数量，做个记录，方便以后查询。


# 解决办法

下面以ext4为例子


<pre>
#以下操作/dev/sdaX数据将会全部丢失
mkfs.ext4 -N 希望的Inode数 /dev/sdaX
blkid /dev/sdaX
vim /etc/fstab#修改新UUID
mount -a
</pre>

that's all，enjoy！！

参考blog：

[http://www.centoscn.com/CentOS/config/2013/1017/1872.html](http://www.centoscn.com/CentOS/config/2013/1017/1872.html )
