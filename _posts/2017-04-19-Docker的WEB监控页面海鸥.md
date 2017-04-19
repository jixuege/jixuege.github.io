---
layout: post
title: Docker的WEB监控页面海鸥
date: 2017-04-19
categories: blog
tags: [监控,docker]
description: Docker的WEB监控页面海鸥
---


## 前言
今天无意中又发现了一个dockers的监控工具，叫做海鸥。

## 具体介绍及操作

<pre>
一条命令搞定。就是这么任性。
docker run -d -p 10086:10086 -v /var/run/docker.sock:/var/run/docker.sock tobegit3hub/seagull
</pre>
IE浏览器输入：ip:10086即可


更新完毕！！enjoy！！

