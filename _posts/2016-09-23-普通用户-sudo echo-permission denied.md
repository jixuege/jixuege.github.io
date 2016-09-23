---
layout: post
title: sudo echo "hehe" > /usr/share/nginx/html/index.html提示权限不够
date: 2016-9-23
categories: blog
tags: [错误总结，sudo]
description: 在利用sudo执行sudo echo的时候出现提示权限不够，但是我已经给了这个权限了。
---

# 前言

使用普通用户sudo echo 执行重定向命令的时候提示权限不够，已经在/etc/sudoers下做了配置。
www    ALL=(ALL)    NOPASSWD: /usr/bin/echo
# 解决办法

在使用sudo echo 'hehe'>/usr/local/index.html的时候，其实sudo只用在了echo 上，而重定向没有用到sudo的权限，所以会提示“权限不够”。解决办法如下：

方法一：

<pre>

sudo sh -c "echo hehe > /usr/share/nginx/html/index.html "
注意这个地方的sh也要给sudo 权限
</pre>

方法二：
<pre>
下面相当于>，会替换到index.html里的内容
echo "dd"|sudo tee  /usr/share/nginx/html/index.html
如果是追加需要加参数-a ,相当于>>
echo "dd"|sudo tee -a /usr/share/nginx/html/index.html
</pre>

that's all，enjoy！！
