---
layout: post
title: CI持续集成系列之（六）pipeline插件的安装及使用
date: 2016-9-12
categories: blog
tags: [持续集成构建发布系列]
description: 安装pipeline更好的查看构建执行先后顺序效果
---

# 前言

上篇已经详细介绍了，如何通过插件来实现两个项目之间的关联，今天呢，就要介绍另外一个优秀的插件pipeline来图形化看到构建执行的全过程。

# 具体操作

## 插件安装

插件名称叫做pipeline plugin ，可以插件安装也可以上传插件安装。

* 方法一：
<pre>
cd /var/lib/jenkins/plugins
rz pipeline-input-step.jpi  pipeline-rest-api.jpi  pipeline-stage-step.jpi  pipeline-stage-view.jpi
systemctl restart jenkins
</pre>

* 方法二：

安装插件
![rt](http://7xwp9m.com1.z0.glb.clouddn.com/六-pipe-插件.png_jixuege)

## 创建pipeline项目

![rt](http://7xwp9m.com1.z0.glb.clouddn.com/6-pipe创建.png_jixuege)
![rt](http://7xwp9m.com1.z0.glb.clouddn.com/6-pipe项目配置.png_jixuege)
点击保存。

## 验证

![如图](http://7xwp9m.com1.z0.glb.clouddn.com/6-最终效果图.png_jixuege)

这样一来就实现了我们想要的可视化执行过程。that's all，enjoy！！