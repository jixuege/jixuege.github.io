---
layout: post
title: 手把手教你用samba来创建windows下的文件共享
date: 2016-9-19
categories: blog
tags: [文件共享,samba]
description: Linux服务器上利用samba来创建windows下的文件共享服务器
---

# 前言 #

有一天，领导说，XX，给我们部门单独来个文件共享吧，把一些常用的服务放进来，这样，以后大家需要什么软件，比如visor ，亿图之类的可以执行在这个地方找。好吧，所以有了下面这篇在Linux服务器上利用samba来创建windows下的文件共享服务器。

# 具体实践 #
当前系统：centos 6
安装软件
<pre>
yum install samba samba-client samba-swat
</pre>
检查服务是否安装
<pre>
[root@localhost samba]#  rpm -qa | grep samba
samba-3.6.23-36.el6_8.x86_64
samba-winbind-clients-3.6.23-36.el6_8.x86_64
samba-winbind-3.6.23-36.el6_8.x86_64
samba-common-3.6.23-36.el6_8.x86_64
</pre>
启动服务
<pre>
[root@localhost samba]# /etc/init.d/smb start
</pre>
启动 SMB 服务：
设置开机启动项
<pre>
[root@localhost samba]#  chkconfig --level 35 smb on
[root@localhost samba]#  chkconfig --list | grep smb
smb            	0:关闭	1:关闭	2:关闭	3:启用	4:关闭	5:启用	6:关闭
</pre>

配置文件，匿名共享
在配置文件/etc/samba/smb.conf
添加一项和注释家目录
<pre>
#============================ Share Definitions ==============================
;[homes]
;	comment = Home Directories
;	browseable = no
;	writable = yes
;	valid users = %S
;	valid users = MYDOMAIN\%S
[public]
	comment = 技术共享
	path = /share
	public = yes
;	writable = yes    #注释这个其他window不能修改其中文件
</pre>
重启服务生效
<pre>
[root@localhost samba]# /etc/init.d/smb restart
关闭 SMB 服务：                                            [确定]
启动 SMB 服务：                                            [确定]
</pre>
验证
共享目录是/share
![rutu](http://7xwp9m.com1.z0.glb.clouddn.com/samba-1.jpg_jixuege)
![rutu](http://7xwp9m.com1.z0.glb.clouddn.com/samba-2.png_jixuege)
其他配置参考：[http://www.centoscn.com/CentosServer/ftp/2015/0318/4915.html](http://www.centoscn.com/CentosServer/ftp/2015/0318/4915.html)
that's all ，enjoy！！


