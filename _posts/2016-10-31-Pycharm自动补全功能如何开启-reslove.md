---
layout: post
title: Pycharm开启自动补全功能
date: 2016-10-31
categories: blog
tags: [Pycharm，error]
description: Pycharm开启自动补全功能
---

# 前言

在使用Pycharm写Python代码的时候写到关键字的时候，不会自动出现默认词了，Google了一下，终于找到问题了。

# 解决办法

把下图这个地方取消即可。这个功能叫做：节能模式。
![rutu](http://7xwp9m.com1.z0.glb.clouddn.com/QQ图片20161031091550.png_jixuege)


具体表现就是：关闭后，Pycharm就跟文本编辑器差不多了，不会去关联上下文，像纠错，联想等关键字等功能都没了。

PS：

接着说一个如何实现CTRL+鼠标滚轮实现字体的放大和缩小。

File --> Setting --> Editor --> General --> 勾选Change font size (zoom) with Ctrl+Mouse Wheel

that's all，enjoy！！

参考blog：

[http://www.jianshu.com/p/62ad97edb6f3](http://www.jianshu.com/p/62ad97edb6f3)

