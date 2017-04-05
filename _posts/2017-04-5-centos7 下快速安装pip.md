---
layout: post
title: Centos7下快速安装pip
date: 2017-04-5
categories: blog
tags: [python,centos7]
description: Centos7下快速安装pip
---


## 前言

Centos7下快速安装pip工具

## 具体介绍及操作
<pre>
1、安装epel扩展源
yum -y install epel-release

2、安装
yum -y install python-pip

3、安装Django
pip install Django
提示如下：
Successfully installed Django-1.11 pytz-2017.2
You are using pip version 8.1.2, however version 9.0.1 is available.
You should consider upgrading via the 'pip install --upgrade pip' command.
4、我很听话，执行下面命令升级即可。
pip install --upgrade pip
</pre>

更新完毕！！enjoy！！