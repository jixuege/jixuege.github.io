---
layout: post
title: 手把手教你手动安装Jenkins插件
date: 2016-9-14
categories: blog
tags: [插件，Jenkins]
description: 常用的2个插件的安装，Gitlab Hook Plugin和Gitlab Plugin
---

# 前言
  终于把Jenkins安装好了，发现安装最简单的插件都非常费力。总是提示失败，常用的2个插件为Gitlab Plugin（从gitlab拉取代码）和Gitlab Hook Plugin（提交代码到Gitlab触发自动构建）。
下面就介绍如何手动安装这2个插件，亲测有效。

# 具体操作
其实很简单，跟把大象放到冰箱步骤是一样的，也是分为3部分。

第一步：下载插件
下载Gitlab Plugin地址:[http://updates.jenkins-ci.org/latest/gitlab-plugin.hpi](http://updates.jenkins-ci.org/latest/gitlab-plugin.hpi)

下载Gitlab Hook Plugin：

这个需要下载2个插件：

gitlab-hook:

[http://updates.jenkins-ci.org/latest/gitlab-hook.hpi](http://updates.jenkins-ci.org/latest/gitlab-hook.hpi)

ruby-runtime:

[http://updates.jenkins-ci.org/latest/ruby-runtime.hpi](http://updates.jenkins-ci.org/latest/ruby-runtime.hpi)

第二步：把下载好的插件放到Jenkins机器目录下/var/lib/jenkins/plugins


第三步：重启Jenkins服务

<pre>
[root@jenkins-99 plugins]# systemctl restart jenkins
</pre>

that's all，enjoy！！