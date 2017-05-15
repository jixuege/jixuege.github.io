---
layout: post
title: Centos7上快速安装MySQL
date: 2017-05-4
categories: blog
tags: [mysql,linux]
description: 如何在Centos7上快速安装MySQL
---

## 前言

Centos7上快速安装MySQL
## 具体介绍

<pre>
从最新版本的centos系统开始，默认的是 Mariadb而不是mysql！
使用系统自带的repos安装很简单：
yum install mariadb mariadb-server
systemctl start mariadb ==> 启动mariadb
systemctl enable mariadb ==> 开机自启动
mysql_secure_installation ==> 设置 root密码等相关,比如密码给123456
mysql -uroot -p123456 ==> 测试登录！
给远程登录权限
把在所有数据库的所有表的所有权限赋值给位于所有IP地址的root用户。

mysql> grant all privileges on *.* to root@'%'identified by '123456';
如果是新用户而不是root，则要先新建用户

mysql>create user 'username'@'%' identified by 'password'; 
此时就可以进行远程连接了。

结束！

</pre>
