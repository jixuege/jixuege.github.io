---
layout: post
title: Harbor 修改仓库地址
date: 2017-03-30
categories: blog
tags: [仓库,Docker]
description: 修改harbor的访问方式
---


## 前言

以前使用harbor是以IP的方式来进行，现在想利用域名来访问，顺便测试一下在别的地方进行下载

## 具体介绍及操作

执行下面操作：

1、修改配置文件
<pre>
vi harbor.cfg
hostname = registry.cloud.jyall.com


</pre>
2、重启服务
<pre>
[root@node1-222 harbor]# find . -name  docker-compose.yml
./make/dev/docker-compose.yml
[root@node1-222 harbor]# cd make/dev/

docker-compose stop
docker-compose rm -f
docker-compose up -d

</pre>

如何使用呢？如果docker版本是1.12以上的，那么就需要进行修改仓库地址, 把框的地方写上你的仓库域名地址，比如www.harbortest.com
![rt](http://7xwp9m.com1.z0.glb.clouddn.com/I1.png_jixuege)

还需要公开仓库，不公开就需要先登录然后在pull。登入方法如下：

<pre>
docker login registry.cloud.jyall.com 
输入用户名和密码

</pre>
如何去公开仓库呢？URL输入仓库地址，找到你新建的仓库，公开即可，如下图
![rt](http://7xwp9m.com1.z0.glb.clouddn.com/写的.png_jixuege)

然后直接pull即可
<pre>
docker pull registry.cloud.jyall.com/jyall_pre/java:3.2
</pre>
记住如果要上传就需要tag前面为项目名称。
Harbor要求第一个/后面的为项目名称，要求Harbor中存在这个项目名称，否则就会报 错误

更改完毕！！enjoy！！

