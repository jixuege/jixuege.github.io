e---
layout: post
title: Centos7下yum安装软件报错解决办法
date: 2017-04-2
categories: blog
tags: [NFS,centos7]
description: Centos7下升级使用yum报错
  File "/usr/bin/yum", line 29, in <module>
---


## 前言

机器初始化之后使用yum安装软件报错
<pre>
Traceback (most recent call last):
  File "/usr/bin/yum", line 29, in <module>
    yummain.user_main(sys.argv[1:], exit_code=True)
  File "/usr/share/yum-cli/yummain.py", line 365, in user_main
    errcode = main(args)
  File "/usr/share/yum-cli/yummain.py", line 271, in main
    return_code = base.doTransaction()
  File "/usr/share/yum-cli/cli.py", line 773, in doTransaction
    resultobject = self.runTransaction(cb=cb)
  File "/usr/lib/python2.7/site-packages/yum/__init__.py", line 1736, in runTransaction
    if self.fssnap.available and ((self.conf.fssnap_automatic_pre or
  File "/usr/lib/python2.7/site-packages/yum/__init__.py", line 1126, in <lambda>
    fssnap = property(fget=lambda self: self._getFSsnap(),
  File "/usr/lib/python2.7/site-packages/yum/__init__.py", line 1062, in _getFSsnap
    devices=devices)
  File "/usr/lib/python2.7/site-packages/yum/fssnapshots.py", line 158, in __init__
    self._vgnames = _list_vg_names() if self.available else []
  File "/usr/lib/python2.7/site-packages/yum/fssnapshots.py", line 56, in _list_vg_names
    names = lvm.listVgNames()
lvm.LibLVMError: (0, '')

</pre>


## 具体介绍及操作
解决办法
<pre>
yum clean metadata
yum clean all
yum update
最后一步很关键，就是要重启
reboot
</pre>



更新完毕！！enjoy！！

