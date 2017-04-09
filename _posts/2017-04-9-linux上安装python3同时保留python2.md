---
layout: post
title: linux上安装python3同时保留python2
date: 2017-04-9
categories: blog
tags: [python,centos7]
description: linux上安装python3同时保留python2 
---


## 前言

linux上安装python3同时保留python2？这个就要用到上篇说到的path变量了。
## 具体介绍及操作
这里我下载python3.6版本来进行介绍
<pre>
django默认数据库为sqlite3，所以安装下面这个很有必要。
yum install sqlite-devel
下载包
wget https://www.python.org/ftp/python/3.6.0/Python-3.6.0.tgz
解压
tar xf Python-3.6.0.tgz
配置安装信息
 ./configure --prefix=/usr/local/python3/
编译
make && make install
配置环境变量
新建文件
vim /etc/profile.d/python3.sh 
export PATH=$PATH:/usr/local/python3/bin/
执行一下下面命令
export PATH=$PATH:/usr/local/python3/bin/
验证
python3
Python 3.6.0 (default, Feb  1 2017, 14:56:52) 
[GCC 4.8.5 20150623 (Red Hat 4.8.5-11)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 

</pre>

更新完毕！！enjoy！！