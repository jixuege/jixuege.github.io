---
layout: post
title: 手把手教你使用Pycharm连接Github
date: 2016-9-24
categories: blog
tags: [工具，pycharm]
description: 手把手教你使用Pycharm连接Github
---

# 前言

使用pycharm来连接github做代码管理

# 具体操作

## 第一步：绑定账号

File->Settings->Version Control->Github
会出现github，然后在旁边输入你github的用户名和密码，可以点击”test”测试一下，如果出现：Connection successful,则代表连接成功了。

![1](http://7xwp9m.com1.z0.glb.clouddn.com/2016-9-24-pycharm-github.png_jixuege)

## 第二步：选择 git安装路径

设置github后选择git，输入你git.exe的安装位置路径,下面是我的git.exe的位置,然后点击OK。

![2](http://7xwp9m.com1.z0.glb.clouddn.com/2016-9-24-git地址.png_jixuege)

## 第三步： 创建github的仓库

VCS->Import Into Version Control->Share Project On Github

![3](http://7xwp9m.com1.z0.glb.clouddn.com/2016-9-24-github仓库.png_jixuege)

然后会弹出框让你输入一个仓库名（不能为中文）,填写完后点击share

![4](http://7xwp9m.com1.z0.glb.clouddn.com/2016-9-24-仓库名.png_jixuege)

然后会弹出让你选择哪些文件需要被同步，选好后，在下面的commit Message可以输入自己的信息，然后点OK，你的代码就提交到网上了。可以看看

![5](http://7xwp9m.com1.z0.glb.clouddn.com/2016-9-24-commit.png_jixuege)

这样你的github上面就有了你创建的仓库和刚才提交的文件

![6](http://7xwp9m.com1.z0.glb.clouddn.com/2016-9-24-git仓库.png_jixuege)

## 创建文件并提交

![7](http://7xwp9m.com1.z0.glb.clouddn.com/2016-9-24-推送测试.png_jixuege)


![8](http://7xwp9m.com1.z0.glb.clouddn.com/2016-9-24-推送确定.png_jixuege)

## 查看验证

![9](http://7xwp9m.com1.z0.glb.clouddn.com/2016-9-24-验证.png_jixuege)

that's all，enjoy！！