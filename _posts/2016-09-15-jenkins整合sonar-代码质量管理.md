---
layout: post
title: CI持续集成系列之（四）持续代码质量管理-Sonar部署及使用
date: 2016-9-15
categories: blog
tags: [持续集成构建发布系列]
description: 持续代码质量管理-Sonar部署及使用并结合jenkins使用
---

# 前言
首先，我们要知道什么是sonar？
简单的理解就是，开发写的代码，我用它来做一个检测。

>详细介绍如下：
sonar是一个用于代码质量管理的开放平台。通过插件机制，Sonar可以集成不同的测试工具，代码分析工具以及持续集成工具。与持续集成工具(例如Hudson/Jenkins等)不同，Sonar并不是简单地把不同的代码检查工具结果(例如FindBugs，PDM等)直接显示在Web页面上，而是通过不同的插件对这些结果进行再加工处理，通过量化的方式度量代码的变化，从而可以方便地对不同规模和种类的工程进行代码质量管理 
在对其他工具的支持方面，Sonar不仅提供了对IDE的支持，可以在Eclipse和IntelliJIDEA这些工具里联机查看结果；同时Sonar还对大量的持续集成工具提供了接口支持，可以很方便地在持续集成中使用Sonar。此外，Sonar的插件还可以对java以外的其他编程语言提供支持，对国际化及报告文档化也有良好的支持。

# 具体操作
>快速熟悉一个软件，是把它先安装上，然后运行起来。

1、当前环境
<pre>
[root@gitlab-102 ~]# cat /etc/redhat-release 
CentOS Linux release 7.2.1511 (Core)
</pre>

2、sonar部署
<pre>
#依赖java环境
# yum install -y java-1.8.0
# cd /usr/local/src
# wget https://sonarsource.bintray.com/Distribution/sonarqube/sonarqube-5.6.zip
# unzip sonarqube-5.6.zip
# mv sonarqube-5.6 /usr/local/
# ln -s /usr/local/sonarqube-5.6/ /usr/local/sonarqube
</pre>

3、安装数据库并配置
>Sonar还需要安装mysql数据库(5.6以上)
<pre>
# rpm -ivh https://mirror.tuna.tsinghua.edu.cn/mysql/yum/mysql57-community-el6/mysql-community-release-el6-7.noarch.rpm
#yum install mysql-server -y
# /etc/init.d/mysqld start
# mysql
mysql> CREATE DATABASE sonar CHARACTER SET utf8 COLLATE utf8_general_ci;
mysql> GRANT ALL ON sonar.* TO 'sonar'@'localhost' IDENTIFIED BY 'sonar@pw';
mysql> GRANT ALL ON sonar.* TO 'sonar'@'%' IDENTIFIED BY 'sonar@pw';
mysql> FLUSH PRIVILEGES;
# cd /usr/local/sonarqube/conf
#修改配置文件如下
# grep "^[a-Z]" sonar.properties 
sonar.jdbc.username=sonar
sonar.jdbc.password=sonar@pw
sonar.jdbc.url=jdbc:mysql://localhost:3306/sonar?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true&useConfigs=maxPerformance
sonar.web.host=0.0.0.0
sonar.web.port=9000

</pre>

4、启动sonar
<pre>
# /usr/local/sonarqube/bin/linux-x86-64/sonar.sh start
</pre>

5、IE输入http://10.0.0.102:9000/看是否能正常访问，默认用户名和密码都是admin，英文界面不是很友好，下面就把他汉化。

6、汉化
<pre>
#cd /usr/local/sonarqube/extensions/plugins
# wget https://github.com/SonarQubeCommunity/sonar-l10n-zh/releases/download/sonar-l10n-zh-plugin-1.11/sonar-l10n-zh-plugin-1.11.jar
# /usr/local/sonarqube/bin/linux-x86-64/sonar.sh restart
</pre>

7、插件安装
>这个很容易理解，sonar对代码的分析是通过插件来完成的，机分析java代码就需要安装java插件，分析php呢就需要安装php插件。那我们想如何去分析代码呢？这个就需要代码扫描器sonar-scanner了。

* 安装php和python插件
![安装插件](http://7xwp9m.com1.z0.glb.clouddn.com/四-sonar安装插件.gif)

* 安装扫描器插件
>注意这个扫描器要安装在jenkins服务器上
<pre>
# cd /usr/local/src/
# wget https://sonarsource.bintray.com/Distribution/sonar-scanner-cli/sonar-scanner-2.6.1.zip 
# unzip sonar-scanner-2.6.1.zip -d /usr/local/
#ln -s /usr/local/sonar-scanner-2.6.1/ /usr/local/sonar-scanner
配置让扫描器跟sonar关联起来
# cd 
# cd /usr/local/sonar-scanner/conf/
# grep "^[a-Z]" sonar-scanner.properties 
sonar.host.url=http://10.0.0.102:9000
sonar.sourceEncoding=UTF-8
sonar.jdbc.username=sonar
sonar.jdbc.password=sonar@pw
sonar.jdbc.url=jdbc:mysql://10.0.0.102:3306/sonar?useUnicode=true&amp;characterEncoding=utf8

</pre>

* 下载测试代码来进行测试
<pre>
# cd
# wget https://github.com/SonarSource/sonar-examples/archive/master.zip
# unzip master.zip
# cd /root/sonar-examples-master/projects/languages/php/php-sonar-runner
# /usr/local/sonar-scanner/bin/sonar-scanner
</pre>
>上面是以PHP为例：
需要注意的是，每个语言项目中都需要有这样一个配置文件，只有有这个配置以后才能进行扫描
<pre>
[root@gitlab-102 php]# pwd
/root/sonar-examples-master/projects/languages/php
[root@gitlab-102 php]# cat php-sonar-runner/sonar-project.properties 
# Required metadata
sonar.projectKey=org.sonarqube:php-simple-sq-scanner
#项目名称
sonar.projectName=PHP :: Simple Project :: SonarQube Scanner
#版本号
sonar.projectVersion=1.0

# Comma-separated paths to directories with sources (required)
#代码目录
sonar.sources=src

#语言格式
# Language
sonar.language=php
# 编码
# Encoding of the source files
sonar.sourceEncoding=UTF-8
</pre>
* 分析结果：
![结果](http://7xwp9m.com1.z0.glb.clouddn.com/四-sonar扫描结果.png)
可以点进去查看详情信息。

8、结合jenkins实现拉取代码后自动检测
>那么jenkins如何调用sonarqube呢，当然就是装插件咯。
* 安装sonar插件
![插件](http://7xwp9m.com1.z0.glb.clouddn.com/四-sonar插件.png)
如果这个插件安装不上，一样的，下载插件上传到/var/lib/jenkins/plugins下，重启jenkins服务即可。

* jenkins关联sonar
上面插件安装以后，依次执行下面操作：

系统管理==>系统设置==>找到SonarQube servers
![tp](http://7xwp9m.com1.z0.glb.clouddn.com/四-sonar配置.png_jixuege)
点击保存！！
>需要注意的是，扫描工具需要安装在jenkins服务器上，因为代码拉下来以后就是在jenkins服务器了。

* 添加扫描器
系统管理==> global Tool ==>
![全局配置](http://7xwp9m.com1.z0.glb.clouddn.com/四-sonar全局配置.jpg_jixuege)
点击保存！！

9、新建一个项目，然后结合jenkins和sonar检测实践

* 创建jenkins与gitlab建立免密钥（使用root作为免密钥）
<pre>
# ssh-keygen -t rsa
# cat .ssh/id_rsa.pub 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDhKvUnz9WPB3LYKTxcJEOe1cyM7IkUzY2u91U0RL/yVBFrREVsOc9KQyMt+hY1w0oWuZ5YG1qNvdaIXKYuNa7xzztXmodYXZKawu2ulQcv5yN3zH3XxFF5ox9FiZ0g5afUqDKVS0Xlyz1YHKpYt25z5FsSpeVPUDrYLUwFPuBdOG6oNXIIKrZaBs/3Fy6HTEDpycegipU3T9S95R48stPeXcQmzCzAYyUHeJk2ye4g7oOXRJysrbdz5edeTwTiwzLSa8zXMyAlpsNV4RuuEIQ/qf+rP7LnAO2s9EA5FhPAfUUqf6jVapvGNAgJN7zjpV0AIQFZjVJU+SuVMhAbeSxB root@jenkins-101

</pre>

* 新建一个项目，并拷贝上面的公钥到项目下的key
![图](http://7xwp9m.com1.z0.glb.clouddn.com/四-新建项目拷贝公钥1.gif)
* 登陆jenkins创建公钥用户
![tt](http://7xwp9m.com1.z0.glb.clouddn.com/四-新建jenkins用户.gif)

* 创建项目并关联sonar执行验证结果
![验证](http://7xwp9m.com1.z0.glb.clouddn.com/四-验证.gif)
里面有填写sonar的配置文件，如下
<pre>
# Required metadata
sonar.projectKey=demo
sonar.projectName=demo
sonar.projectVersion=2.0
# Comma-separated paths to directories with sources (required)
sonar.sources=./
# Language
sonar.language=php
# Encoding of the source files
sonar.sourceEncoding=UTF-8
</pre>
>上面我们已经说过，如果没有这个配置文件，不会去扫描，一般都是跟代码放在一起，这里我只是做了一个测试，所以单独写出来了。

10、配置如果构建失败发送邮件
这里只需要配置一个地方即可。
![收件人邮箱](http://7xwp9m.com1.z0.glb.clouddn.com/四-收件邮箱地址.jpg_jixuege)

11、验证

关闭gitlab服务，然后构建项目，看是否能收到邮件
<pre>
[root@gitlab-102 php-sonar-runner]# gitlab-ctl stop
</pre>
构建服务，查看配置的收件邮箱是否有邮件，如果有说明关联成功，如果没有，检查配置的邮件服务器是否有问题。

that's all，enjoy！！