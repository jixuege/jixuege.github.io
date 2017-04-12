---
layout: post
title: Centos 7 上harbor仓库访问从HTTP换成HTTPS
date: 2017-04-12
categories: blog
tags: [harbor,centos7]
description:  Centos 7 上harbor仓库访问从HTTP换成HTTPS
---


## 前言

Centos 7 上harbor仓库访问从HTTP换成HTTPS

## 具体介绍及操作
首先，要说明一下我们上传后的镜像在物理机的哪个地方？
Harbor的默认镜像存储路径在/data/registry目录下，映射到docker容器里面的/storage目录下。这个参数是在docker-compose.yml中指定的，在docker-compose up -d运行之前修改。如果希望将Docker镜像存储到其他的磁盘路径，可以修改这个参数。
###Configuring Harbor with HTTPS Access
<pre>
这里以IP 10.10.20.227为例。利用IP地址生成证书
openssl req -newkey rsa:4096 -nodes -sha256 -keyout ca.key -x509 -days 365 -out ca.crt

openssl req  -newkey rsa:4096 -nodes -sha256 -keyout 10.10.20.227.key -out 10.10.20.227.csr

echo subjectAltName = IP:10.10.20.227 > extfile.cnf

openssl x509 -req -days 365 -in 10.10.20.227.csr -CA ca.crt -CAkey ca.key -CAcreateserial -extfile extfile.cnf -out 10.10.20.227.crt

mkdir -p /root/cert
cp 10.10.20.227.crt /root/cert/
cp 10.10.20.227.key /root/cert/
cd /data/harbor
vim harbor.cfg
 #set hostname
  hostname = 10.10.20.227
  #set ui_url_protocol
  ui_url_protocol = https
  ......
  #The path of cert and key files for nginx, they are applied only the protocol is set to https 
  ssl_cert = /root/cert/10.10.20.227.crt
  ssl_cert_key = /root/cert/10.10.20.227.key
初始化配置
[root@docker harbor]# ./prepare
停掉服务
[root@docker harbor]# docker-compose down  
启动服务
[root@docker harbor]# docker-compose up –d

浏览器访问Harbor管理入口，输入填写的IP地址即可。https://10.10.20.227/
用户名和密码是：admin/Harbor12345
</pre>

更新完毕！！enjoy！！

参考blog：https://github.com/vmware/harbor/blob/master/docs/configure_https.md
