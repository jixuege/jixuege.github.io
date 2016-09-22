---
layout: post
title: CI持续集成系列之（五）Jenkins多项目的关联
date: 2016-8-14
categories: blog
tags: [持续集成构建发布系列]
description: Jenkins多项目的关联，例如把sonar和发布项目管理起来
---

# 前言
前面我们介绍了gitlab仓库安装，Jenkins的安装，Jenkins插件安装，sonar代码质量检测，接下来我们就要说说如何关联Jenkins的多个项目。

# 具体操作

首先，我们要这么想，我如何把两个没关系的项目管理起来，达到这一一个目的，就是A执行完以后接着去执行B？Jenkins同样提供了这样的插件，Parameterized Trigger plugin。直接在插件里面安装。
![rutu](http://7xwp9m.com1.z0.glb.clouddn.com/五-Jenkins-parameterized插件.png_jixuege)

由于前面已经创建了一个sonar代码检测项目，现在我们在来创建一个项目，用这个插件来关联，项目名为demo-deploy。

![rutu](http://7xwp9m.com1.z0.glb.clouddn.com/五-Jenkins-parameterized插件-创建项目.png_jixuege)

创建完毕后，回到web-demo代码质量管理项目，选择 配置，新增下面一项：
![rutu](http://7xwp9m.com1.z0.glb.clouddn.com/五-Jenkins-关联-配置.png_jixuege)

或者（经过测试效果是一样的）
![rt](http://7xwp9m.com1.z0.glb.clouddn.com/五-Jenkins关联配置2.png_jixuege)

点击构建，可以看到web-demo执行完以后就开始执行demo-deploy。如下图：
![rutu](http://7xwp9m.com1.z0.glb.clouddn.com/五-Jenkins-parameterized-关联.png_jixuege)

这样一来，关联项目就已经实现了。that's all，enjoy！！