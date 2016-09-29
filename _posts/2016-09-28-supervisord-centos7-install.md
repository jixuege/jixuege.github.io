---
layout: post
title: CentOS7 上安装supervisord服务
date: 2016-9-28
categories: blog
tags: [supervisord，服务搭建]
description: 如何在centos 7上搭建supervisor
---

#  前言

在做自动化集成的时候，由于一些启动命令需要用到supervisord 来管理，所以就需要使用supervisor，这里顺便就写一下如何在centos 7上搭建supervisor

# 具体操作

## 1、安装supervisor

<pre>
pip install supervisor
安装好supervisor之后，默认是没有生成配置文件的。可以通过下面命令来生成
echo_supervisord_conf > /etc/supervisord.conf
 mkdir /etc/supervisord.d/
</pre>

## 2、修改配置文件

<pre>
vim /etc/supervisord.conf，在其中加入如下：
[include]
files = /etc/supervisord.d/*.conf

此处的/etc/supervisord.d/用于存放各种program的supervisord启动脚本（后缀为conf）。
*.conf配置文件格式如下：注意命令需要自己改写，并且要放在前台执行
cat /etc/supervisord/xx.conf
[program:containeragent]
command = 这里填写启动服务命令
autostart=true    
autorestart=true  
startsecs=3
然后添加Supervisor的service控制命令：
vim /usr/lib/systemd/system/supervisord.service，并输入：
[Unit]
Description=Supervisord
[Service]
Type=forking
PIDFile=/tmp/supervisord.pid
ExecStart=/usr/bin/supervisord -c /etc/supervisord.conf
ExecStop=/bin/kill -TERM $MAINPID
ExecReload=/bin/kill -HUP $MAINPID
[Install]
WantedBy=multi-user.target
</pre>

## 3、启动服务

<pre>
systemctl start supervisord.service
systemctl enable supervisord.service
</pre>

that's all，enjoy！！

参考blog地址：[https://www.v2ex.com/t/144370](https://www.v2ex.com/t/144370)