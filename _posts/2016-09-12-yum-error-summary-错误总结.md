---
layout: post
title: python2.7升级到3.5之后yum无法使用解决办法
date: 2016-9-12
categories: blog
tags: [错误，总结]
description: python2.7升级到3.5之后yum无法使用的解决办法
---

# 前言

Python2.7升级以后到3.5出现yum无法使用问题，接下来会介绍python2.7升级过程及yum无法使用的解决办法。


# 具体操作
1、 基础环境准备

- 系统信息
<pre>
# cat /etc/redhat-release 
CentOS Linux release 7.2.1511 (Core) 
# uname -r
3.10.0-327.22.2.el7.x86_64
</pre>

- 查看当前版本号
<pre>
# python --version
Python 2.7.5
</pre>


2、Python升级

- 准备编译环境
<pre>
yum groupinstall 'Development Tools'
yum install zlib-devel bzip2-devel  openssl-devel ncurses-devel
</pre>

- 下载Python3.5代码包并编译安装
<pre>
cd /usr/local/src
wget  https://www.python.org/ftp/python/3.5.0/Python-3.5.0.tar.xz
tar xvf  Python-3.5.0.tar.xz
cd Python-3.5.0
./configure --prefix=/usr/local/python3.5
make && make install
</pre>

- 移除原来的Python并做软链接
<pre>
mv /usr/bin/python /usr/bin/pythonbak
ln -s /usr/local/python3.5/bin/python3.5 /usr/bin/python
</pre>

- 更新yum配置
<pre>
[root@jumpserver Python-3.5.0]# ll /usr/bin | grep python
-rwxr-xr-x.   1 root root       11216 Dec  1  2015 abrt-action-analyze-python
lrwxrwxrwx    1 root root          34 Sep 12 15:51 python -> /usr/local/python3.5/bin/python3.5
lrwxrwxrwx    1 root root           9 Aug 27 16:39 python2 -> python2.7
-rwxr-xr-x    1 root root        7136 Aug 18 23:59 python2.7
-rwxr-xr-x    1 root root        1835 Aug 18 23:58 python2.7-config
lrwxrwxrwx    1 root root          16 Aug 27 16:39 python2-config -> python2.7-config
lrwxrwxrwx    1 root root           7 Aug 27 16:39 pythonbak -> python2
lrwxrwxrwx    1 root root          14 Aug 27 16:39 python-config -> python2-config

</pre>
修改如下：
<pre>
vim /usr/bin/yum
通过vim修改yum的配置
#!/usr/bin/python改为#!/usr/bin/python2.7，保存退出。
完成了python3的安装。
</pre>

- 验证yum能否正常使用
<pre>
[root@jumpserver Python-3.5.0]# yum install lrzsz
Loaded plugins: fastestmirror, langpacks
Loading mirror speeds from cached hostfile
 * epel: mirrors.neusoft.edu.cn
  File "/usr/libexec/urlgrabber-ext-down", line 28
    except OSError, e:
                  ^
SyntaxError: invalid syntax
Exiting on user cancel
</pre>
> 提示这个文件问题

查看发现这个文件也依赖Python版本，修改为相同即可。修改如下：
<pre>
vim /usr/libexec/urlgrabber-ext-down
#! /usr/bin/python2.7
</pre>

到此，升级和解决yum问题已解决！enjoy！！