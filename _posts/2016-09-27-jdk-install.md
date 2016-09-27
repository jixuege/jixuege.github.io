---
layout: post
title: CentOS上yum安装和卸载JDK
date: 2016-9-27
categories: blog
tags: [环境配置，Java环境]
description: CentOS上yum安装和卸载JDK
---

#  前言

主要讲解如何正确搜索包并安装的过程。

# 具体操作

## 1、查看可用的Java包
<pre>
[root@bc0b2bc996d6 /]# yum search java | grep -i --color jdk
ldapjdk-javadoc.noarch : Javadoc for ldapjdk
icedtea-web.x86_64 : Additional Java components for OpenJDK - Java browser
java-1.6.0-openjdk.x86_64 : OpenJDK Runtime Environment
java-1.6.0-openjdk-demo.x86_64 : OpenJDK Demos
java-1.6.0-openjdk-devel.x86_64 : OpenJDK Development Environment
java-1.6.0-openjdk-javadoc.x86_64 : OpenJDK API Documentation
java-1.6.0-openjdk-src.x86_64 : OpenJDK Source Bundle
java-1.7.0-openjdk.x86_64 : OpenJDK Runtime Environment
java-1.7.0-openjdk-accessibility.x86_64 : OpenJDK accessibility connector
java-1.7.0-openjdk-demo.x86_64 : OpenJDK Demos
java-1.7.0-openjdk-devel.x86_64 : OpenJDK Development Environment
java-1.7.0-openjdk-headless.x86_64 : The OpenJDK runtime environment without
java-1.7.0-openjdk-javadoc.noarch : OpenJDK API Documentation
java-1.7.0-openjdk-src.x86_64 : OpenJDK Source Bundle
java-1.8.0-openjdk.x86_64 : OpenJDK Runtime Environment
java-1.8.0-openjdk-accessibility.x86_64 : OpenJDK accessibility connector
java-1.8.0-openjdk-accessibility-debug.x86_64 : OpenJDK accessibility connector
java-1.8.0-openjdk-debug.x86_64 : OpenJDK Runtime Environment with full debug on
java-1.8.0-openjdk-demo.x86_64 : OpenJDK Demos
java-1.8.0-openjdk-demo-debug.x86_64 : OpenJDK Demos with full debug on
java-1.8.0-openjdk-devel.x86_64 : OpenJDK Development Environment
java-1.8.0-openjdk-devel-debug.x86_64 : OpenJDK Development Environment with
java-1.8.0-openjdk-headless.x86_64 : OpenJDK Runtime Environment
java-1.8.0-openjdk-headless-debug.x86_64 : OpenJDK Runtime Environment with full
java-1.8.0-openjdk-javadoc.noarch : OpenJDK API Documentation
java-1.8.0-openjdk-javadoc-debug.noarch : OpenJDK API Documentation for packages
java-1.8.0-openjdk-src.x86_64 : OpenJDK Source Bundle
java-1.8.0-openjdk-src-debug.x86_64 : OpenJDK Source Bundle for packages with
ldapjdk.noarch : The Mozilla LDAP Java SDK
openprops.noarch : An improved java.util.Properties from OpenJDK
</pre>

可以看到有这么2个包

JDK7

<pre>
java-1.7.0-openjdk.x86_64 : OpenJDK Runtime Environment
java-1.7.0-openjdk-devel.x86_64 : OpenJDK Development Environment
</pre>

JDK8

<pre>
java-1.8.0-openjdk.x86_64 : OpenJDK Runtime Environment
java-1.8.0-openjdk-devel.x86_64 : OpenJDK Development Environment
</pre>

## 2、安装JRE和JDK

<pre>
yum -y install java-1.7.0-openjdk.x86_64
yum -y install java-1.7.0-openjdk-devel.x86_64
</pre>

centOS上JDK默认安装路径为/usr/lib/jvm

<pre>
[root@bc0b2bc996d6 jvm]# pwd
/usr/lib/jvm
[root@bc0b2bc996d6 jvm]# ll
total 4
lrwxrwxrwx 1 root root   26 Mar 25 08:44 java -> /etc/alternatives/java_sdk
lrwxrwxrwx 1 root root   32 Mar 25 08:44 java-1.7.0 -> /etc/alternatives/java_sdk_1.7.0
lrwxrwxrwx 1 root root   40 Mar 25 08:44 java-1.7.0-openjdk -> /etc/alternatives/java_sdk_1.7.0_openjdk
drwxr-xr-x 8 root root 4096 Mar 25 08:44 java-1.7.0-openjdk-1.7.0.95-2.6.4.0.el7_2.x86_64
lrwxrwxrwx 1 root root   34 Mar 25 08:44 java-openjdk -> /etc/alternatives/java_sdk_openjdk
lrwxrwxrwx 1 root root   21 Mar 25 08:42 jre -> /etc/alternatives/jre
lrwxrwxrwx 1 root root   27 Mar 25 08:42 jre-1.7.0 -> /etc/alternatives/jre_1.7.0
lrwxrwxrwx 1 root root   35 Mar 25 08:42 jre-1.7.0-openjdk -> /etc/alternatives/jre_1.7.0_openjdk
lrwxrwxrwx 1 root root   52 Mar 25 08:42 jre-1.7.0-openjdk-1.7.0.95-2.6.4.0.el7_2.x86_64 -> java-1.7.0-openjdk-1.7.0.95-2.6.4.0.el7_2.x86_64/jre
lrwxrwxrwx 1 root root   29 Mar 25 08:42 jre-openjdk -> /etc/alternatives/jre_openjdk
</pre>

## 3、配置Java环境

<pre>
vi /etc/environment
#写入JAVA_HOME、CLASSPATH
JAVA_HOME=/usr/lib/jvm/open-jdk
CLASSPATH=.:$JAVA_HOME/lib:$JAVA_HOME/jre/lib
PATH=$PATH:$JAVA_HOME/bin
source /etc/environment
echo $JAVA_HOME
java -version
</pre>

## 4、卸载JDK

查看当前系统的JDK
<pre>
[root@bc0b2bc996d6 jvm]# rpm -qa|grep jdk
</pre>

卸载
<pre>
yum -y remove java java-1.7.0-openjdk-devel.x86_64
</pre>
完毕，到此为止，讲解了Java1.7和Java1.8的安装及卸载的全过程。

当然你也可以用最简单的方式来安装Java环境：

<pre>
yum install -y java-1.8.0
</pre>
这么安装也没问题，看你自己了。

that's all，enjoy！！