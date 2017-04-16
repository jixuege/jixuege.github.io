---
layout: post
title: 【已解决】WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED
date: 2017-04-15
categories: blog
tags: [publickey,Linux]
description:  【已解决】WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!
---


## 前言
拷贝公钥到其他服务器出现下面报错信息：
<pre>
-bash-4.2$ ssh-keygen -R 10.10.20.127
/var/lib/jenkins/.ssh/known_hosts updated.
Original contents retained as /var/lib/jenkins/.ssh/known_hosts.old
-bash-4.2$ ssh-copy-id -i .ssh/id_rsa.pub root@10.10.20.220
/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/bin/ssh-copy-id: ERROR: @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
ERROR: @    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
ERROR: @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
ERROR: IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
ERROR: Someone could be eavesdropping on you right now (man-in-the-middle attack)!
ERROR: It is also possible that a host key has just been changed.
ERROR: The fingerprint for the ECDSA key sent by the remote host is
ERROR: 3a:5c:5a:3f:15:2c:89:2d:f0:8c:d4:90:7c:2a:f8:fb.
ERROR: Please contact your system administrator.
ERROR: Add correct host key in /var/lib/jenkins/.ssh/known_hosts to get rid of this message.
ERROR: Offending ECDSA key in /var/lib/jenkins/.ssh/known_hosts:4
ERROR: ECDSA host key for 10.10.20.220 has changed and you have requested strict checking.
ERROR: Host key verification failed.
</pre>


## 具体介绍及操作
解决办法如下:

删除本机.ssh/known_hosts,然后在重新分发一下
<pre>
bash-4.2$ rm -f .ssh/known_hosts
-bash-4.2$ ssh-copy-id -i .ssh/id_rsa.pub root@10.10.20.220
The authenticity of host '10.10.20.220 (10.10.20.220)' can't be established.
ECDSA key fingerprint is 3a:5c:5a:3f:15:2c:89:2d:f0:8c:d4:90:7c:2a:f8:fb.
Are you sure you want to continue connecting (yes/no)? yes 

</pre>

更新完毕！！enjoy！！

