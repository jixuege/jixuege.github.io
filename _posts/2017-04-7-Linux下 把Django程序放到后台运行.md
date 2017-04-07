---
layout: post
title: Linux下把Django程序放在后台运行
date: 2017-04-7
categories: blog
tags: [Django,centos7]
description: Linux下把Django程序放在后台运行 
---


## 前言

linux下centos7环境，需求是把Django程序放在后台运行
## 具体介绍及操作
<pre>
方法一：
进入到项目目录下
[root@docker ~]# cd /usr/local/src/new_cmdb/

运行下面程序
nohup python manage.py runserver 0.0.0.0:9000 &


方法二：这个比较高级，使用screen
1、安装screen
yum install -y screen

2、新建一个screen
screen -S xiedi
这样会新开一个窗口，然后执行命令
python manage.py runserver 0.0.0.0:9000即可

3、重开一个窗口，列出所有screen进程，如下
[root@docker ~]# screen -ls
There are screens on:
	3029.xiedi	(Attached)

4、如果想链接上这个会话，执行命令
screen -r 3029即可
</pre>

更新完毕！！enjoy！！