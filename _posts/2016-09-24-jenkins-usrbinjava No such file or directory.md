---
layout: post
title: centos7启动Jenkins报错java: No such file or directory
date: 2016-9-24
categories: blog
tags: [错误总结，jenkins]
description: 在centos7上启动Jenkins时提示bash: /usr/bin/java: No such file or directory的解决办法
---

# 前言

在centos 7下，启动Jenkins时出现报错，提示Starting Jenkins bash: /usr/bin/java: No such file or directory

# 问题描述

报错信息如下：

<pre>
# systemctl status jenkins.service
● jenkins.service - LSB: Jenkins Continuous Integration Server
   Loaded: loaded (/etc/rc.d/init.d/jenkins)
   Active: failed (Result: exit-code) since Tue 2016-08-23 21:40:35 CST; 9s ago
     Docs: man:systemd-sysv-generator(8)
  Process: 32061 ExecStop=/etc/rc.d/init.d/jenkins stop (code=exited, status=0/SUCCESS)
  Process: 32355 ExecStart=/etc/rc.d/init.d/jenkins start (code=exited, status=1/FAILURE)
Aug 23 21:40:35 docker20-127 systemd[1]: Starting LSB: Jenkins Continuous Integration Server...
Aug 23 21:40:35 docker20-127 runuser[32356]: pam_unix(runuser:session): session opened for user jenkins by (uid=0)
Aug 23 21:40:35 docker20-127 jenkins[32355]: Starting Jenkins bash: /usr/bin/java: No such file or directory
Aug 23 21:40:35 docker20-127 runuser[32356]: pam_unix(runuser:session): session closed for user jenkins
Aug 23 21:40:35 docker20-127 jenkins[32355]: [FAILED]
Aug 23 21:40:35 docker20-127 systemd[1]: jenkins.service: control process exited, code=exited status=1
Aug 23 21:40:35 docker20-127 systemd[1]: Failed to start LSB: Jenkins Continuous Integration Server.
Aug 23 21:40:35 docker20-127 systemd[1]: Unit jenkins.service entered failed state.
Aug 23 21:40:35 docker20-127 systemd[1]: jenkins.service failed.
</pre>

# 解决办法

<pre>
# echo $JAVA_HOME
/usr/local/jdk
 which java
/usr/local/jdk/bin/java
# ln -s /usr/local/jdk/bin/java /usr/bin/java
启动成功
service jenkins start
Starting jenkins (via systemctl):                          [  OK  ]
</pre>

that's all，enjoy！！
