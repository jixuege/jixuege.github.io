---
layout: post
title: 手把手教你用nginx搭建文件服务器
date: 2016-9-20
categories: blog
tags: [文件服务器,nginx]
description: 讲解一步一步搭建简单的文件服务器
---

# 前言 #
搭建一个简单的文件服务器，最简单的就是利用nginx来发布，占用端口80，如需更改，修改配置文件即可。


# 实战

当前环境及IP地址
<pre>
# cat /etc/redhat-release 
CentOS Linux release 7.2.1511 (Core) 
# uname -r
3.10.0-327.el7.x86_64
# getenforce 
Permissive
# systemctl status firewalld
# ifconfig eth0|awk -F "[ :]+" 'NR==2 {print $3}'
10.10.20.221
# hostname
jy-file
</pre>
配置阿里云的epel 源
<pre>
# wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
</pre>
安装 nginx
<pre>
#yum install nginx -y
</pre>
修改配置文件
查看配置文件位置：
<pre>
#rpm -qc nginx
/etc/logrotate.d/nginx
/etc/nginx/fastcgi.conf
/etc/nginx/fastcgi.conf.default
/etc/nginx/fastcgi_params
/etc/nginx/fastcgi_params.default
/etc/nginx/koi-utf
/etc/nginx/koi-win
/etc/nginx/mime.types
/etc/nginx/mime.types.default
/etc/nginx/nginx.conf
/etc/nginx/nginx.conf.default
/etc/nginx/scgi_params
/etc/nginx/scgi_params.default
/etc/nginx/uwsgi_params
/etc/nginx/uwsgi_params.default
/etc/nginx/win-utf
</pre>
修改配置文件如下：
<pre>
#cat /etc/nginx/nginx.conf
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;
events {
    worker_connections 1024;
}
http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    access_log  /var/log/nginx/access.log  main;
    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;
    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;
    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf;
    server {
        listen       80 ;
        server_name  file.cloud.jyall.com;
	charset utf-8;
        # Load configuration files for the default server block.
	
        location / {
		root  /data;
		  index  index.php index.html index.htm;
		 autoindex on;
		 autoindex_exact_size on;
	autoindex_localtime on;
        }
	access_log  /var/log/file.log;
        error_page 404 /404.html;
            location = /40x.html {
        }
        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }
}
</pre>

指定目录路径为/data/,创建点文件
<pre>
#cd /data/
#ll
总用量 4
drwxr-xr-x. 2 root root 4096 8月  25 10:01 autoconf
drwxr-xr-x. 2 root root   61 8月  24 17:16 template_name
-rw-r--r--. 1 root root    0 8月  23 11:10 test
</pre>
浏览器输入IP地址即可查看验证。

that's all，enjoy！！
