---
layout: post
title: Centos7下升级Python版本到3.3
date: 2017-04-2
categories: blog
tags: [NFS,centos7]
description: Centos7下升级Python版本到3.3
---


## 前言

熟悉LinuxCentos7升级Python版本到3.3


## 具体介绍及操作

<pre>
1、下载Python包
# wget http://www.python.org/ftp/python/3.3.0/Python-3.3.0.tgz
# tar xvf Python-3.3.0.tgz
2、新建一个目录，做软连接使用
mkdir /usr/local/python3
# cd Python-3.3.0
注意要安装gcc
# yum install gcc -y
./configure --prefix=/usr/local/python3
# make && make install
3、此时没有覆盖老版本，再将原来/usr/bin/python链接改为别的名字
mv /usr/bin/python /usr/bin/python_old
4、再建立新版本python的链接
ln -s /usr/local/python3/bin/python3 /usr/bin/python

5、修改/usr/bin/yum的第一行为：
#!/usr/bin/python_old
就可以了

</pre>



更新完毕！！enjoy！！

