---
layout: post
title: Linux下运行Django项目报错提示no module named_sqlite3
date: 2017-04-6
categories: blog
tags: [python,centos7]
description: Linux下运行Django项目报错提示no module namedsqlite3 
---


## 前言

linux下centos7环境，升级Python版本到3.5.2，运行Django程序，提示报错信息no module named_sqlite3

## 具体介绍及操作
<pre>
1、安装sqlite-devel
yum install sqlite-devel -y

2、记住要重新编译一下Python，这是关键
[root@docker ~]# cd Python-3.5.2
[root@docker Python-3.5.2]# make && make install


3、安装Django
下载包，地址如下：
https://www.djangoproject.com/download/
解压安装
tar xzvf Django-X.Y.tar.gz    # 解压下载包
cd Django-X.Y                # 进入 Django 目录
python setup.py install      # 执行安装命令

4、运行Django
python manage.py runserver

</pre>

更新完毕！！enjoy！！