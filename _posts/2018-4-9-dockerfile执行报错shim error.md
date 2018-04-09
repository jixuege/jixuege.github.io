---
layout: post
title: dockerfile执行报错shim error docker-runc not installed on system
date: 2018-4-9
categories: blog
tags: [Linux]
description: dockerfile执行报错shim error docker-runc not installed on system 的解决办法
---


## 前言
执行dockerfile，然后报错信息如下：shim error： docker-runc not installed on system


## 具体介绍及操作

解决办法如下：
<pre>
cd /usr/libexec/docker/
ln -s docker-runc-current docker-runc
查看
# ll
总用量 7384
-rwxr-xr-x 1 root root  820472 3月   8 01:07 docker-init-current
-rwxr-xr-x 1 root root 1687304 3月   8 01:07 docker-proxy-current
lrwxrwxrwx 1 root root      19 4月   9 17:32 docker-runc -> docker-runc-current
-rwxr-xr-x 1 root root 5047808 3月   8 01:07 docker-runc-current

</pre>

that's all！！
