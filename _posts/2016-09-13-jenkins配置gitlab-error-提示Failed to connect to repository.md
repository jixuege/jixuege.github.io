---
layout: post
title: Jenkins配置gitlab地址报错"Failed to connect to repository..."
date: 2016-9-13
categories: blog
tags: [总结,错误]
description: Jenkins配置gitlab地址报错"Failed to connect to repository : Error performing command: git ls-remote -h https://10.10.20.100/python/Python-project.git HEAD"
---

# 前言

在Jenkins上创建项目，配置gitlab地址报错，报错信息如下图：
![如图](http://7xwp9m.com1.z0.glb.clouddn.com/error.png_jixuege)

# 解决办法

这个是由于没有认证的缘故，需要在拷贝Jenkins机器上的公钥到gitlab上，具体操作如下：
Jenkins服务器上操作
<pre>
# ssh-keygen -t dsa
#cat ~/.ssh/id_dsa.pub
把上面的公钥拷贝到gitlab上即可。
</pre>
拷贝位置，按照下面操作即可。
![如图](http://7xwp9m.com1.z0.glb.clouddn.com/拷贝公钥.gif)

问题解决，enjoy！！
