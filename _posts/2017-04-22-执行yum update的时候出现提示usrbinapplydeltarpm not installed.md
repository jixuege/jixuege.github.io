---
layout: post
title: 执行yum update的时候出现提示usr bin applydeltarpm not installed
date: 2017-04-22
categories: blog
tags: [error,yum]
description: 执行yum update的时候出现提示usrbinapplydeltarpm not installed
---

## 前言
更新yum的时候出现下面提示：
Delta RPMs disabled because /usr/bin/applydeltarpm not installed

## 具体介绍及操作

解决办法如下：
<pre>
 yum install deltarpm

如果没有这个把，可以尝试安装
 yum provides '*/applydeltarpm'
</pre>


更新完毕！！enjoy！！

