---
layout: post
title: Centos 上ruby1.9.2的安装
date: 2016-9-27
categories: blog
tags: [环境配置，ruby环境]
description: 在Centos上安装ruby1.9.2版本
---

# 前言

为何会有这篇博文呢？原起于ruby版本不一致导致报错。默认的ruby版本为1.8比较旧。所以，这篇就演示如何安装ruby1.9.2版本。

# 具体操作

这个下载以1.9为主，如果想使用最新版本可以再次下载包：[最新版本点我](http://www.ruby-lang.org/zh_TW/downloads/)
<pre>
# wget ftp://ftp.ruby-lang.org//pub/ruby/1.9/ruby-1.9.2-p0.tar.gz
# tar -zxvf ruby-1.9.2-p0.tar.gz
# cd ruby-1.9.2-p0
# ./configure
# make && make install
</pre>

上面操作完以后呢，执行下面操作：
<pre>
[root@CentOS6 ruby-1.9.2-p0]# ruby -v
-bash: /usr/bin/ruby: No such file or directory
</pre>

这是由于新版本装在/usr/local/bin下，而yum方式安装的会在/usr/bin底下，所以改变一下$PATH即可。
<pre>
export PATH=/usr/local/bin:$PATH
# ruby -v
ruby 1.9.2p0 (2010-08-18 revision 29036) [x86_64-linux]

</pre>

that's all，enjoy！！

参考blog：[https://blog.wu-boy.com/2011/10/install-ruby-rubygems-compass-on-centos/](https://blog.wu-boy.com/2011/10/install-ruby-rubygems-compass-on-centos/)