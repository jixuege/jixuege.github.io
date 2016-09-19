---
layout: post
title: 手把手教你搭建梯子
date: 2016-9-17
categories: blog
tags: [梯子,搭建]
description: 手把手教你搭建梯子
---

# 前言 #

 最近在国外买了一个VPS，主要目的就是用来搭建梯子，好上谷歌，购买的地址为：[http://www.vultr.com/?ref=6894149](http://www.vultr.com/?ref=6894149)。通过此链接注册用户，然后充值10$，就会赠送50$。然后在开一个服务器5$一个月的，基本上就可以使用1年了。好了，不多少了，开始了！！

搭建SS具体如下：
推荐秋水逸冰，好像有一直在维护这个脚本，还有就是python版的，以前用shell一键安装，由于操作系统不一致还有需要修改脚本，这个就不用，直接安装即可
参考地址如下：[https://teddysun.com/342.html/comment-page-21](https://teddysun.com/342.html/comment-page-21)

附上操作过程：

# 第一步 #
我的系统是centOS7
<pre>
[root@jixuege ~]# cat /etc/redhat-release 
CentOS Linux release 7.2.1511 (Core) 
</pre>
root登陆
<pre>
wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh
chmod +x shadowsocks.sh
./shadowsocks.sh 2>&1 | tee shadowsocks.log
</pre>

安装完成以后会提示下面信息，表示成功
<pre>
Congratulations, shadowsocks install completed!
Your Server IP:your_server_ip
Your Server Port:your_server_port
Your Password:your_password
Your Local IP:127.0.0.1
Your Local Port:1080
Your Encryption Method:aes-256-cfb
Welcome to visit:https://teddysun.com/342.html
Enjoy it!
</pre>

卸载软件的方法如下：
<pre>
./shadowsocks.sh uninstall
</pre>

单用户配置文件路径为：/etc/shadowsocks.json
<pre>
{
    "server":"0.0.0.0",
    "server_port":8989,
    "local_address":"127.0.0.1",
    "local_port":1080,
    "password":"yourpassword",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
</pre>
我正在使用的多用户配置如下：
<pre>
[root@jixuege ~]# cat /etc/shadowsocks.json
{
 "server":"0.0.0.0",
 "local_address": "127.0.0.1",
 "local_port":1080,
  "port_password": {
     "8381": "密码",
     "8382": "密码",
     "8383": "密码",
     "8989": "密码",
     "8384": "密码"
 },
 "timeout":300,
 "method":"aes-256-cfb",
 "fast_open": false
}
</pre>
接下来执行命令即可：
<pre>
启动：/etc/init.d/shadowsocks start
停止：/etc/init.d/shadowsocks stop
重启：/etc/init.d/shadowsocks restart
状态：/etc/init.d/shadowsocks status
</pre>

# 第二步 #
然后呢，用免费的锐速加速SS
这里还要提醒一点，就是如果VPS是Openvz是装不上锐速的，使用下面的方法就可以检测出来
<pre>
wget -N --no-check-certificate https://raw.githubusercontent.com/91yun/code/master/vm_check.sh && bash vm_check.sh
</pre>

代码运行结束就会在最后一行显示虚拟化技术：kvm还是openv或者是xen一目了然。
它会显示下面信息
![tizi](http://7xwp9m.com1.z0.glb.clouddn.com/梯子.png_jixuege)

由于我的内核不兼容，必须更换内核才能安装
教程：CentOS更换内核，提供锐速可用的内核下载：[http://www.91yun.org/archives/795](http://www.91yun.org/archives/795)
CentOS7内核更换为： 3.10.0-229.1.2.el7.x86_64
<pre>
rpm -ivh http://soft.91yun.org/ISO/Linux/CentOS/kernel/kernel-3.10.0-229.1.2.el7.x86_64.rpm --force
</pre>
查看内核是否安装成功
<pre>
rpm -qa | grep kernel
</pre>
重启生效
<pre>
reboot
uname -r
</pre>
然后安装的时候会提示让你选择一个相近的安装即可。
安装完成以后呢
执行下面操作：
<pre>
wget http://my.serverspeeder.com/d/ls/serverSpeederInstaller.tar.gz
tar xzvf serverSpeederInstaller.tar.gz
bash serverSpeederInstaller.sh
</pre>
默认设置直接回车即可，然后其他选y即可。
安装完以后，还可以进行一些优化设置：

<pre>
vi /serverspeeder/etc/config
rsc="1"，RSC网卡驱动模式
advinacc="1"  流量方向加速
maxmode="1"  最大传输模式
</pre>

下面重新启动锐速的服务
<pre>
停止
/serverspeeder/bin/serverSpeeder.sh stop
启动
/serverspeeder/bin/serverSpeeder.sh start
</pre>
参考地址：[http://bbs.tcp.hk/thread-41-1-1.html](http://bbs.tcp.hk/thread-41-1-1.html)

#第三步

接下来就是下载客户端了
下载地址：[https://github.com/shadowsocks/shadowsocks-windows/releases](https://github.com/shadowsocks/shadowsocks-windows/releases)
把你的服务端口密码IP都写进去即可，如下图
![如图](http://7xwp9m.com1.z0.glb.clouddn.com/2.png_jixuege)

到这里，基本就可以google了， 不过如果你还想装个B，那么接着往下看。

# 第四步 
我有加了一个看线路路由的装逼神奇Best Trace，下载地址在这里：[http://www.ipip.net/download.html](http://www.ipip.net/download.html)
安装上以后，可以看看我的网站路由走向了，如下图
![ss](http://7xwp9m.com1.z0.glb.clouddn.com/ss.png_jixuege)

是不是猴赛雷的样子。
参考地址：[http://www.91yun.org/archives/1699](http://www.91yun.org/archives/1699) 完结！！！










