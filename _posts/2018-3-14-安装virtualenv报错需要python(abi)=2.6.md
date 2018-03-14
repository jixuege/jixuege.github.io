---
layout: post
title: 安装virtualenv报错需要python(abi)=2.6
date: 2018-3-14
categories: blog
tags: [Linux]
description: 安装virtualenv报错需要python(abi)=2.6
---


## 前言
安装virtualenv报错需要python(abi)=2.6
<pre>
执行了
yum install -y python-virtualenv
报错

解决依赖关系完成
错误：软件包：python-virtualenv-12.0.7-1.el6.noarch (epel)
          需要：python(abi) = 2.6
          已安装: python-2.7.5-58.el7.x86_64 (@base)
              python(abi) = 2.7
              python(abi) = 2.7
          可用: python34-3.4.5-4.el6.i686 (epel)
              python(abi) = 3.4
 您可以尝试添加 --skip-broken 选项来解决该问题
 您可以尝试执行：rpm -Va --nofiles --nodigest
</pre>

## 具体介绍及操作

解决办法如下：

<pre>
换一种安装方式，不过前提是你要有pip工具
yum install python-pip -y
pip install virtualenv

</pre>

that's all！！
