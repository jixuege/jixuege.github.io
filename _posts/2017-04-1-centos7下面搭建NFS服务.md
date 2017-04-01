---
layout: post
title: Centos7下安装NFS服务
date: 2017-04-1
categories: blog
tags: [NFS,centos7]
description: Centos7下安装NFS服务
---


## 前言

熟悉Linux下centos 7下面NFS服务搭建


## 具体介绍及操作
服务端：
<pre>
1、安装软件包
yum install -y nfs-utils
2、编辑exports文件
vim /etc/exports
/data 192.168.56.100(rw,sync,fsid=0)  192.168.56.101(rw,sync)

这样，这2台机器就可以挂载NFS服务器上的/data目录到文件系统，其中
rw表示可读写，sync表示同步写

3、启动NFS服务
systemctl enable rpcbind.service
systemctl enable nfs-server.service
启动服务
systemctl start rpcbind.service
systemctl start nfs-server.service
确定NFS服务启动
rpcinfo -p
检查NFS服务器是否挂载了共享目录
exportfs
#可以查看到已经ok
/data           192.168.56.100
/data           192.168.56.101

4、安装NFS客户端
yum install -y nfs-utils
systemctl enable rpcbind.service
systemctl start rpcbind.service
注意：如果只是客户端，不需要启动NFS服务

5、检查NFS服务端是否有目录共享
showmount -e nfs服务器的IP

6、挂载
创建目录
mkdir /data
挂载
mount -t nfs4 nfs服务器IP:/    /data
查看
df -h

7、加入开机启动项
vi /etc/fstab
# 加上
nfs服务器IP:/   /data  nfs ro,hard,intr,proto=tcp,port=2049,noauto 0 0

</pre>



更新完毕！！enjoy！！

