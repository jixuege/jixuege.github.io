---
layout: post
title: 手把手教你用Tomcat搭建Jenkins服务
date: 2016-9-29
categories: blog
tags: [Jenkins，服务搭建]
description: 利用Tomcat来搭建Jenkins持续集成环境
---

#  前言

我们知道Jenkins的作用和意义，今天要说的是在docker容器里利用tomcat来搭建Jenkins环境


# 具体操作


## 1、运行一个容器

<pre>
docker run -i -t -p 8080:8080 -p 8101:8101 -p 9001:9001 --name='jenkins' centos /bin/bash
</pre>

## 2、进入容器并安装环境


<pre>
docker exec -it jenkins bash
yum -y install java-1.8.0-openjdk git
yum install wget -y 
mkdir /opt/jenkins
cd /opt/jenkins
wget http://apache.fayea.com/tomcat/tomcat-8/v8.0.36/bin/apache-tomcat-8.0.36.tar.gz
tar xzf  apache-tomcat-8.0.36.tar.gz
cd /opt/jenkins/apache-tomcat-8.0.36/webapps/
wget http://mirrors.jenkins-ci.org/war/latest/jenkins.war
rm -rf ROOT
mv jenkins.war ROOT.war   #这样的目的就是在启动tomcat之后，他会解压这个包，生产一个ROOT的目录，这样我们直接访问http://localhost:8080/ 就可以了，而不是还需要指定目录

</pre>

## 3、启动服务


<pre>
#/opt/jenkins/apache-tomcat-8.0.36/bin/startup.sh
</pre>

## 4、访问验证


本地浏览器访问：http://localhost:8080/ ，可以看到jenkins的界面了。剩下的操作可以参考[Jenkins的简单安装](http://www.jixuege.com/blog/2016/08/11/CI-jenkins-%E7%AE%80%E5%8D%95%E5%AE%89%E8%A3%85-2/)

that's all，enjoy！！
