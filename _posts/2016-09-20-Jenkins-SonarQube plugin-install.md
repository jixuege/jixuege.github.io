---
layout: post
title: 手动安装Jenkins插件SonarQube Plugin
date: 2016-9-20
categories: blog
tags: [jenkins,插件]
description: 讲解手动安装插件，原因在于自己安装过程中一直失败，做一个记录，以免重犯。
---

# 前言
我们知道Jenkins最强大功能就是插件，在结合Jenkins实现拉取代码后自动检测代码质量，这个时候如果Jenkins想调用sonarqube就需要安装插件SonarQube Plugin。接下来讲解的是如何手动安装，因为在Jenkins页面一直安装失败。

# 实操
按照正常做法，我们是会在Jenkins下插件里面搜索安装，如下图所示
![如图](http://7xwp9m.com1.z0.glb.clouddn.com/%E5%9B%9B-sonar%E6%8F%92%E4%BB%B6.png_jixuege)
由于本人安装多次，依旧失败，所以放弃了，还是手动安装吧。

由于一直忽略了安装SonarQube Plugin 还需要安装别的插件，所以一直手动安装不成功。下面就记录下来，避免大家走弯路。

这里需要3个插件：

分别是

- javadoc.hpi： [http://updates.jenkins-ci.org/download/plugins/javadoc/1.4/javadoc.hpi](http://updates.jenkins-ci.org/download/plugins/javadoc/1.4/javadoc.hpi)
- jquery.hpi： [http://updates.jenkins-ci.org/download/plugins/jquery/1.11.2-0/jquery.hpi](http://updates.jenkins-ci.org/download/plugins/jquery/1.11.2-0/jquery.hpi)
- sonar.hpi：  [http://updates.jenkins-ci.org/download/plugins/sonar/2.4.4/sonar.hpi](http://updates.jenkins-ci.org/download/plugins/sonar/2.4.4/sonar.hpi)

下载完以后，上传到/var/lib/jenkins/plugins/下面然后重启Jenkins服务：
<pre>
# systemctl restart jenkins
</pre>

如何验证呢？

在系统管理==>系统设置==>如果可以找到SonarQube servers，就说明插件已安装成功。如下图所示：
![http://7xwp9m.com1.z0.glb.clouddn.com/%E5%9B%9B-sonar%E9%85%8D%E7%BD%AE.png_jixuege](http://7xwp9m.com1.z0.glb.clouddn.com/%E5%9B%9B-sonar%E9%85%8D%E7%BD%AE.png_jixuege)

that's，enjoy！！