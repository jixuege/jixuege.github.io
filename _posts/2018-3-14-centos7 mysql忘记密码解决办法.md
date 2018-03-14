---
layout: post
title: centos7 mysql忘记密码解决办法
date: 2018-3-14
categories: blog
tags: [Linux]
description: centos7 mysql忘记密码解决办法
---


## 前言
centos7 安装mysql以及忘记密码解决办法


## 具体介绍及操作

解决办法如下：
<pre>
mysql 安装过程
wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
rpm -ivh mysql-community-release-el7-5.noarch.rpm
yum install mysql-server -y
systemctl start mysql
systemctl enable mysql
systemctl status mysql

</pre>

修改密码过程
<pre>
vi /etc/my.cnf

在[mysqld]中添加

skip-grant-tables

重启mysql

systemctl restart mysql

用户无密码登录

mysql -uroot -p (直接点击回车，密码为空)
选择数据库

use mysql;

修改root密码

update user set authentication_string=password('123') where user='root';

执行

 flush privileges;

退出

exit;

删除配置文件里添加的项

skip-grant-tables

重启mysql

systemctl restart mysql

</pre>

that's all！！
