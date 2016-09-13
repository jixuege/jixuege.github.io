---
layout: post
title: CI持续集成系列之（三）Jenkins的邮件配置及插件使用
date: 2016-9-12
categories: blog
tags: [持续集成构建发布系列]
description: 3-Jenkins的邮件配置及插件使用及跟gitlab关联拉取代码等系列操作
---

# 前言
下面介绍的就是一些入门级别的了，包括如何去配置发送邮件，以及安装一些简单的插件去关联gitlab来拉取代码来运行一个 job。

# 具体操作

1、配置邮件通知

* 系统管理==>系统设置
![如图1](http://ww4.sinaimg.cn/large/006eWBRhjw1f7qxx271e2j30os07tjt6.jpg)

* 找到Jenkins Location来添加管理员邮箱
![如图2](http://ww1.sinaimg.cn/large/006eWBRhjw1f7qxy7vkthj30mb03h0tb.jpg)

* 配置收邮件地址
![如图3](http://ww2.sinaimg.cn/large/006eWBRhjw1f7qxyyhjnrj30n20fbjtm.jpg)

* 保存并查看邮件
![如图4](http://ww4.sinaimg.cn/large/006eWBRhjw1f7qxzyfxe1j30ix05q751.jpg)

* 注意，管理员邮箱需要开启SMTP服务
![如图5](http://ww4.sinaimg.cn/large/006eWBRhjw1f7qy2ltl1kj30m008sdhu.jpg)

具体可见下图
![如图](http://ww3.sinaimg.cn/large/005Dnba3jw1f7r7wn6e79g313f0mjwzp.gif)

2、常用插件安装
![插件安装](http://7xwp9m.com1.z0.glb.clouddn.com/blog3-插件.png_jixuege)
>如果安装不上，可以直接下载对应插件，放在Jenkins机器的目录/var/lib/jenkins/plugins下即可。插件下载地址：[http://updates.jenkins-ci.org/download/plugins/](http://updates.jenkins-ci.org/download/plugins/)

3、添加用户认证，用于拉取gitlab代码
![如图](http://7xwp9m.com1.z0.glb.clouddn.com/1.png_jixuege)

4、拷贝jenkins的公钥到gitlab服务器
Jenkins服务器上操作
<pre>
# ssh-keygen -t dsa
#cat ~/.ssh/id_dsa.pub
把上面的公钥拷贝到gitlab上即可。
</pre>
拷贝位置，按照下面操作即可。
![如图](http://7xwp9m.com1.z0.glb.clouddn.com/拷贝公钥.gif)

5、快速创建一个项目并配置gitlab和拉去代码。
![如图](http://7xwp9m.com1.z0.glb.clouddn.com/快速拉取代码.gif)
that's all，enjoy！！