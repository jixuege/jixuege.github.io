---
layout: post
title: 创建密钥对rsa和dsa到底选谁
date: 2016-9-21
categories: blog
tags: [ssh,密钥对]
description: SSH基于密钥认证，创建密钥对的时候是选rsa还是dsa呢？
---

# 前言
SSH认证在日常的工作中用的是最为频繁的，创建密钥对的时候命令：
<pre>
ssh-keygen -t rsa或者 ssh-keygen -t dsa
</pre>
发现2种生产的密钥对都可使用，那么这两者有和区别呢？接来下就简单聊聊这个事情。

# 区别
我们先看两台机器SSH认证的全过程：

当前有机器node1和node2，现在我想让node1免认证登录node2，操作如下：

* 第一步:生成密钥对（node1上操作）
<pre>
# ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
2f:48:49:c6:50:92:ee:61:e4:db:a5:85:6b:9e:70:9b root@cobbler-10.10.20.248
The key's randomart image is:
+--[ RSA 2048]----+
|    oo.          |
|    o+           |
|   +  +.         |
|    =o..o        |
|   o +o=S        |
|    +.*. .       |
|     =.+. .      |
|      E  .       |
|                 |
+-----------------+
查看生成的密钥对
# ll .ssh/
总用量 12
-rw------- 1 root root 1679 9月  21 10:57 id_rsa  #生成的私钥
-rw-r--r-- 1 root root  407 9月  21 10:57 id_rsa.pub #公钥

</pre>

*  第二步：把公钥拷贝到node2上面,有2种方法
<pre>
方法一：
# ssh-copy-id -i .ssh/id_rsa.pub 10.10.20.248（node2的IP）
这样会在node2的root家目录生成一个叫做authorized_keys，这里面就是node1的公钥
# cat .ssh/authorized_keys
方法二：
在node2的家目录.ssh里手动创建这个authorized_keys文件，里面放置node1的公钥即可。
node2上操作
#cd 
#cd .ssh
#vim authorized_keys
#chmod 600 authorized_keys
</pre>

* 第三步：登录测试
<pre>
# ssh 10.10.20.248（node2的IP）
</pre>

那么生成密钥对我们到底是选择rsa还是dsa呢？参考一些blog给出了这么一些答案。

* 原理与安全性
>两者都是非对称加密算法，RSA的安全性基于极其困难的大整数的分解（两个素数的乘积），而DSA的安全性是基于整数有限域离散对数难题。
基本上认为相同密钥长度的RSA算法和DSA算法安全性相当。但总体来说rsa安全性会更好一点。

* 用途

>dsa 只用于数字签名，RSA即可以作为数字签名也可以作为加密算法 。

* 性能
>相同密钥长度下，dsa做签名速度更快，但做签名验证时速度较慢。

* 业界支持
>rsa更为广泛的部署与支持。

anyway，so，不要纠结，我写这篇就是为了告诉你还是使用ssh-keygen -t rsa 来生成密钥吧。

that's all，enjoy！！

参考blog：[http://blog.sina.com.cn/s/blog_6f31085901015agu.html](http://blog.sina.com.cn/s/blog_6f31085901015agu.html)
