---
layout: post
title: 解决yum doesn't have enough cached data to continue
date: 2016-9-25
categories: blog
tags: [问题总结，Jenkins]
description: 解决yum doesn't have enough cached data to continue
---

# 问题描述

在yum 安装jenkins的时候，出现下面报错
<pre>
One of the configured repositories failed (Unknown),
 and yum doesn't have enough cached data to continue. At this point the only
 safe thing yum can do is fail. There are a few ways to work "fix" this:
     1. Contact the upstream for the repository and get them to fix the problem.
     2. Reconfigure the baseurl/etc. for the repository, to point to a working
        upstream. This is most often useful if you are using a newer
        distribution release than is supported by the repository (and the
        packages for the previous distribution release still work).
     3. Disable the repository, so yum won't use it by default. Yum will then
        just ignore the repository until you permanently enable it again or use
        --enablerepo for temporary usage:
            yum-config-manager --disable <repoid>
     4. Configure the failing repository to be skipped, if it is unavailable.
        Note that yum will try to contact the repo. when it runs most commands,
        so will have to try and fail each time (and thus. yum will be be much
        slower). If it is a very temporary problem though, this is often a nice
        compromise:
            yum-config-manager --save --setopt=<repoid>.skip_if_unavailable=true
Cannot find a valid baseurl for repo: base/7/x86_64
</pre>

刚开始以为是缓存问题，执行下面命令，清除缓存依旧不好使。
<pre>
[root@linux-node1 ~]# yum clean all
[root@linux-node1 ~]# rm -rf /var/cache/yum/
</pre>


# 解决办法

<pre>
#首先备份
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
#下载新的CentOS-Base.repo 到/etc/yum.repos.d/
CentOS 6
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
CentOS 7
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
#之后运行yum makecache生成缓存
</pre>

that's all，enjoy！！
