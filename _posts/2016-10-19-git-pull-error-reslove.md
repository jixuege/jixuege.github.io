---
layout: post
title: git pull时提示Unable to find remote helper for 'https'
date: 2016-10-19
categories: blog
tags: [git，error]
description: git 拉去代码时候提示 find remote helper for https
---

# 前言

我在CentOS 7系统下使用git命令拉去取代码的时候提示
Unable to find remote helper for 'https'
执行的命令如下所示：
<pre>
git clone https://gitlab.jyall.com/jyorder/bs-jyorder-commons.git
</pre>

# 解决办法

找到文件所在目录/usr/libexec/git-core，加入到PATH里面即可。
vim /etc/profile,修改如下
![如图](http://7xwp9m.com1.z0.glb.clouddn.com/path.png)

如何还是提示有问题，可能就差少模块了。
<pre>
yum -y install git-core gitk git-gui 
</pre>

that's all，enjoy！！

参考blog：

[http://blog.csdn.net/yanwuhuan/article/details/7412370](http://blog.csdn.net/yanwuhuan/article/details/7412370)
[http://chenchao40322.blog.51cto.com/2181131/603357](http://chenchao40322.blog.51cto.com/2181131/603357)
