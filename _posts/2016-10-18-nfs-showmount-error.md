---
layout: post
title: nfs showmout提示clnt_create RPC Program not registered
date: 2016-10-18
categories: blog
tags: [nfs，Linux]
description: nfs showmout提示clnt_create: RPC: Program not registered
---

# 前言

nfs服务端在执行命令showmount的时候提示如下：

<pre>
[root@localhost nodiskos]# showmount -e 10.10.20.47
clnt_create: RPC: Program not registered
</pre>

# 解决办法

只需要执行命令rpc.mountd即可

<pre>
[root@localhost nodiskos]# rpc.mountd
[root@localhost nodiskos]# showmount -e 10.10.20.47
Export list for 10.10.20.47:
/home *

</pre>

如果重启nfs服务还是报错，就需要检查一下配置文件了，下面给一个例子：
<pre>
错误示例
[root@localhost ~]# cat /etc/exports
# 这一行是配置默认的工作站系统目录
/nodiskos/workstation　　10.10.20.0/24(rw,async,no_root_squash)
#/nodiskos/workstation 10.10.20.0/24(rw,sync,no_root_squash,no_all_squash)
[root@localhost ~]# systemctl restart nfs
Job for nfs-server.service failed because the control process exited with error code. See "systemctl status nfs-server.service" and "journalctl -xe" for details.
修改配置
[root@localhost ~]# cat /etc/exports
# 这一行是配置默认的工作站系统目录
#/nodiskos/workstation　　10.10.20.0/24(rw,async,no_root_squash)
/nodiskos/workstation 10.10.20.0/24(rw,sync,no_root_squash,no_all_squash)
[root@localhost ~]# systemctl restart nfs

</pre>

还有一个情况，就是nfs服务端启动有先后顺序问题，需要先启动rpcbind服务
<pre>
[root@localhost ~]# systemctl restart rpcbind
[root@localhost ~]# systemctl restart nfs
</pre>

that's all，enjoy！！

参考blog：

[http://blog.csdn.net/hemmingway/article/details/40976351](http://blog.csdn.net/hemmingway/article/details/40976351 )
