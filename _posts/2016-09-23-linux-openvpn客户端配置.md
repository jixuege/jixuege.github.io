---
layout: post
title:Linux（centOS）OpenVPN客户端配置
date: 2016-9-23
categories: blog
tags: [常见服务，openVPN]
description: 在Linux服务器上搭建OpenVPN客户端
---

# 前言

主要讲解如何在Linux服务器上搭建OpenVPN客户端

# 具体操作

说明，当前系统为centos 7 

## 1、添加epel 源
<pre>
rpm -ivh http://mirrors.aliyun.com/epel/epel-release-latest-7.noarch.rpm
</pre>

## 2、安装openVPN

<pre>
# yum install openvpn
</pre>

## 3、加入配置文件

注意：下面的配置文件是在OpenVPN创建的。默认这是空目录/etc/openvpn
<pre>
[root@gitlab-100 ~]# cd /etc/openvpn/
[root@gitlab-100 openvpn]# ll
总用量 20
-rw-r--r--. 1 root root 1695 9月  23 10:48 all.crt
-rw-r--r--. 1 root root 3568 9月  23 16:17 xiedi.ovpn
-rw-r--r--. 1 root root 5314 9月  23 10:47 xiedi.crt
-rw-r--r--. 1 root root 1704 9月  23 10:47 xiedi.key
</pre>

## 启动客户端

<pre>
openvpn /etc/openvpn/xiedi.ovpn &
</pre>

that's all，enjoy！！