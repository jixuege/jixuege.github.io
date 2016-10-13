---
layout: post
title: 解决 syntax error unexpected end of file
date: 2016-10-04
categories: blog
tags: [错误总结，Linux]
description: 执行脚本报错信息syntax error unexpected end of file
---

# 前言

shell脚本执行提示错误信息“syntax error unexpected end of file”，下面就记录一下如何解决。

# 原因分析

这是由于在Windows下写好的shell在Linux上不能直接使用的缘故

# 解决办法

使用命令dos2unix来解决。需要安装一下
<pre>
 yum install dos2unix -y
dos2unix container-entrypoint
</pre>

that's all，enjoy！！
参考blog：

[http://renyongjie668.blog.163.com/blog/static/1600531201172803244846/](http://renyongjie668.blog.163.com/blog/static/1600531201172803244846/ )
