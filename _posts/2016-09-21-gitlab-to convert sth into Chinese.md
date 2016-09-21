---
layout: post
title: 手把手教你Gitlab汉化
date: 2016-9-21
categories: blog
tags: [Gitlab,汉化]
description: 如果不习惯使用英文页面的Gitlab，这也许是你想要的。
---

# 前言

Gitlab我想大家都不陌生了吧，大部分人基本上已经习惯了英文界面，如果你不习惯，那么下面这篇可能是你想要的。手把手教你如何汉化Gitlab。下面汉化过程的gitlab版本号为8.8.5。

# 具体操作
## 当前环境
<pre>
# cat /etc/redhat-release       //系统版本
CentOS Linux release 7.2.1511 (Core)
# uname -r        //内核版本
3.10.0-327.22.2.el7.x86_64
# cat > /etc/yum.repos.d/gitlab-ce.repo << 'EOF'        //添加yum源
[gitlab-ce]
name=gitlab-ce
baseurl=http://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7
repo_gpgcheck=0
gpgcheck=0
enabled=1
gpgkey=https://packages.gitlab.com/gpg.key
EOF
# yum install gitlab-ce-8.8.5-ce.1.el7       //安装gitlab
# vim /etc/gitlab/gitlab.rb
external_url 'http://192.168.56.12'    //修改URL
# gitlab-ctl reconfigure        //配置并启动gitlab
</pre>

## 克隆补丁项目
<pre>
cd
git clone https://gitlab.com/larryli/gitlab.git sourceGitlab
时间有点长，做好心理准备
</pre>

## 得到中文汉化补丁
<pre>
进入目录，把8.8的英文和中文补丁一起diff，得到中文汉化补丁
cd sourceGitlab
git diff origin/8-8-stable origin/8-8-zh > /tmp/8.8.5.diff
gitlab-ctl stop
patch -d /opt/gitlab/embedded/service/gitlab-rails -p1 < /tmp/8.8.5.diff
</pre>

## 验证
![tp](http://7xwp9m.com1.z0.glb.clouddn.com/2016-9-21-gitlab-1.png_jixuege)

that's all，enjoy！！

参考博文：[http://huguiqi.com/2016/07/01/%E5%A6%82%E4%BD%95%E6%B1%89%E5%8C%96gitlab8.9.2%E7%89%88%E6%9C%AC/](http://huguiqi.com/2016/07/01/%E5%A6%82%E4%BD%95%E6%B1%89%E5%8C%96gitlab8.9.2%E7%89%88%E6%9C%AC/)