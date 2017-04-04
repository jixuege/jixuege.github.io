---
layout: post
title: Centos7下docker 指定版本安装
date: 2017-04-4
categories: blog
tags: [docker,centos7]
description: Centos7下docker 指定版本安装
---


## 前言

Centos7下docker 指定版本安装，现在已1.10为例，目前我机器上已经有了docker，不过版本不是我想要的，我先卸载。

## 具体介绍及操作
<pre>
1、卸载当前版本
[root@localhost ~]# rpm -qa|grep docker
docker-client-1.12.6-11.el7.centos.x86_64
docker-common-1.12.6-11.el7.centos.x86_64
docker-1.12.6-11.el7.centos.x86_64
yum remove -y docker-client-1.12.6-11.el7.centos.x86_64   docker-common-1.12.6-11.el7.centos.x86_64

2、安装docker 1.10.3为例
DOCKER_VERSION=1.10.3

# 下载 docker-engine
wget https://yum.dockerproject.org/repo/main/centos/7/Packages/docker-engine-${DOCKER_VERSION}-1.el7.centos.x86_64.rpm

# 下载 docker-engine-selinux
wget https://yum.dockerproject.org/repo/main/centos/7/Packages/docker-engine-selinux-${DOCKER_VERSION}-1.el7.centos.noarch.rpm

yum install -y libtool-ltdl policycoreutils-python

# 先安装 docker-engine-selinux，再安装 docker-engine-selinux
rpm -ivh docker-engine-selinux-${DOCKER_VERSION}-1.el7.centos.noarch.rpm
rpm -ivh docker-engine-${DOCKER_VERSION}-1.el7.centos.x86_64.rpm

# 下面就可以开启docker服务了
systemctl start docker
systemctl enable docker


</pre>

更新完毕！！enjoy！！