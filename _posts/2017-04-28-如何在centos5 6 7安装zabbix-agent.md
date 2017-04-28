---
layout: post
title: 如何在centos 5 6 7 快速安装zabbix agent
date: 2017-04-28
categories: blog
tags: [zabbix,linux]
description: 如何在centos 5 6 7 快速安装zabbix agent
---

## 前言

如何在centos 5 6 7 快速安装zabbix agent,记录一下
## 具体介绍
1、安装源
<pre>
CentOS/RHEL 7:
# rpm -Uvh http://repo.zabbix.com/zabbix/3.0/rhel/7/x86_64/zabbix-release-3.0-1.el7.noarch.rpm

CentOS/RHEL 6:
# rpm -Uvh http://repo.zabbix.com/zabbix/3.0/rhel/6/x86_64/zabbix-release-3.0-1.el6.noarch.rpm

CentOS/RHEL 5:
# rpm -Uvh http://repo.zabbix.com/zabbix/3.0/rhel/5/x86_64/zabbix-release-3.0-1.el5.noarch.rpm

</pre>

2、安装
<pre>
yum install zabbix zabbix-agent

</pre>

3、修改配置文件
<pre>
#Server=[zabbix server ip]
#Hostname=[ Hostname of client system ]

Server=192.168.1.11
Hostname=Server1
</pre>

4、启动服务
<pre>
/etc/init.d/zabbix-agent restart
</pre>
