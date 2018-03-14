---
layout: post
title: RSA host key has changed 报错
date: 2018-3-5
categories: blog
tags: [Linux]
description: RSA host key has changed 报错的解决办法
---


## 前言
在使用scp的时候出现下面报错信息
<pre>
RSA host key for mysharebook.cn has changed and you have requested strict checking.
Host key verification failed.
</pre>

## 具体介绍及操作

解决办法如下：

<pre>
ssh-keygen -R IP  ，IP换成你要连的服务器即可。

</pre>

that's all！！
