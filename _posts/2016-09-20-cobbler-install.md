---
layout: post
title: 解放双手，让我来带你玩转cobbler
date: 2016-9-20
categories: blog
tags: [自动化,cobbler]
description: 讲解一步一步搭建cobbler来实现自动化安装Centos 7和6系统。
---

# 前言 #

首先我要说的是cobbler，它到底是干嘛的？
来，我们先假设一个场景：
假如说你是某公司的一个运维小弟，有一天，你的老大跟你说，我们最近准备搞云了，你看docker多火啊，我们准备买100台机器来搞公有云，你帮我把这些机器全部安装centOS7的系统并优化，这个时候你要怎么做呢？
难道你是一台一台安装吗？如果是，请收下我的膝盖好吗。
这个时候呢，cobbler完美解决你这个问题，让你解放双手，重新做男人！！
没错，装机利器在手，跟我走，天下我有。
官网在此，猛戳有惊喜：[http://cobbler.github.io/](http://cobbler.github.io/)

# 实战

## 第一步：安装cobbler
>cobbler依赖于epel源，所以我们安装epel源

<pre>
rpm -ivh http://mirrors.aliyun.com/epel/epel-release-latest-7.noarch.rpm
它默认会在这里生成一个repo的文件，如下所示
[root@linux-node2 ~]# ll /etc/yum.repos.d/
total 48
-rw-r--r-- 1 root root  957 Mar 31 12:05 epel.repo

</pre>

>yum安装所需服务
<pre>
yum install -y httpd dhcp tftp cobbler cobbler-web pykickstart  xinetd

</pre>

## 第二步：修改settings文件
* 生产一个随机密码，这个用来作为新装机器的root登录密码
<pre>
[root@linux-node2 ~]# openssl passwd -1 -salt  'jixuege' 'jixuege'
$1$jixuege$0VPWaOUa6F0oJtz1HwLgt0
</pre>
注意：
需要复制这个到cobbler的setting里面

* 修改配置文件，有几个地方需要修改
<pre>
# vim /etc/cobbler/settings 
101gg切换到这行：上面随机生成的密码
101 default_password_crypted: "$1$jixuege$0VPWaOUa6F0oJtz1HwLgt0"
242gg :让cobbler来管理DHCP，默认是0关闭的
242 manage_dhcp: 1
272gg:设置TFTP地址
272 next_server: 192.168.56.12
384gg:设置cobbler地址
384 server: 192.168.56.12
：wq保存退出
</pre>

* 修改DHCP分配IP文件
<pre>
# vim /etc/cobbler/dhcp.template 
subnet 192.168.56.0 netmask 255.255.255.0 {
     option routers             192.168.56.2;
     option domain-name-servers 192.168.56.2;
     option subnet-mask         255.255.255.0;
     range dynamic-bootp        192.168.56.100 192.168.56.254;
</pre>

* 修改/etc/xinetd.d/tftp将disable 改为no
<pre>
vim /etc/xinetd.d/tftp
   disable                 = no
</pre>

## 第三步：启动服务
* 启动cobbler服务
<pre>
[root@linux-node2 ~]# systemctl start cobblerd.service
[root@linux-node2 ~]# systemctl enable cobblerd.service
Created symlink from /etc/systemd/system/multi-user.target.wants/cobblerd.service to /usr/lib/systemd/system/cobblerd.service.
[root@linux-node2 ~]# systemctl status cobblerd.service
</pre>

* 启动HTTP服务
<pre>
[root@linux-node2 ~]# systemctl start httpd
[root@linux-node2 ~]# systemctl enable httpd
</pre>

* 启动xinetd服务
<pre>
[root@linux-node2 ~]# systemctl start xinetd
[root@linux-node2 ~]# systemctl enable xinetd
</pre>

* 启动rsyncd服务
<pre>
[root@linux-node2 ~]# systemctl start rsyncd
[root@linux-node2 ~]# systemctl enable rsyncd
</pre>

* 从官网下载bootloader
<pre>
cobbler get-loaders
</pre>

* 检查一下，看是否都完成
<pre>
[root@linux-node2 ~]# cobbler check
The following are potential configuration items that you may want to fix:
1 : debmirror package is not installed, it will be required to manage debian deployments and repositories
2 : fencing tools were not found, and are required to use the (optional) power management features. install cman or fence-agents to use them
Restart cobblerd and then run 'cobbler sync' to apply changes.
</pre>
注意：
上面提示我同步一下，我很听话的
<pre>
[root@linux-node2 ~]# cobbler sync
</pre>

## 第四步：导入镜像和文件
首先，挂载光盘，加入镜像，我这里会加入一个CentOS6和CentOS7的镜像，这里只详细讲一种方法，另外一个自己来吧。
如下图：
![如图](http://7xwp9m.com1.z0.glb.clouddn.com/cobbler-1.png_jixuege)
这样就挂了光盘，然后在Linux里面挂载到/mnt下面

* 挂载CentOS7的系统镜像
<pre>
[root@linux-node2 ~]# mount /dev/cdrom /mnt
</pre>

* 导入镜像
>首先，查看帮助
<pre>
[root@linux-node2 ~]# cobbler import -h
Usage: cobbler [options]
Options:
  -h, --help            show this help message and exit
  --arch=ARCH           OS architecture being imported    指定安装源是多少位
  --breed=BREED         the breed being imported
  --os-version=OS_VERSION
                        the version being imported
  --path=PATH           local path or rsync location     镜像路径
  --name=NAME           name, ex 'RHEL-5'   为安装源定义一个名字
  --available-as=AVAILABLE_AS
                        tree is here, don't mirror
  --kickstart=KICKSTART_FILE
                        assign this kickstart file
  --rsync-flags=RSYNC_FLAGS
                        pass additional flags to rsync

</pre>
注意：安装源的唯一标识就是根据name参数来定义的。
我们来导入7的镜像。
<pre>
[root@linux-node2 ~]# cobbler import --path=/mnt/ --name=CentOS-7-x86_64 --arch=x86_64
task started: 2016-05-28_163256_import
task started (id=Media import, time=Sat May 28 16:32:56 2016)
Found a candidate signature: breed=redhat, version=rhel6
Found a candidate signature: breed=redhat, version=rhel7
Found a matching signature: breed=redhat, version=rhel7
Adding distros from path /var/www/cobbler/ks_mirror/CentOS-7-x86_64:
skipping import, as distro name already exists: CentOS-7-x86_64
No distros imported, bailing out
!!! TASK FAILED !!!
</pre>
提示已经存在，这是正常的，因为存在就回报错，不信我们查看镜像.
<pre>
[root@linux-node2 ~]#cobbler profile list
   CentOS-7-x86_64
</pre>
是的没错，已经存在了。

* 接着我们导入6的镜像，首先卸载，然后导入，查看
<pre>
umount /mnt
[root@linux-node2 ~]# mount /dev/cdrom /mnt
mount: /dev/sr0 is write-protected, mounting read-only
cobbler import --path=/mnt/ --name=CentOS-6-x86_64 --arch=x86_64
[root@linux-node2 ~]# cobbler profile list
   CentOS-6-x86_64
   CentOS-7-x86_64
</pre>

* 导入cfg文件
>进入cobbler的ks..cfg文件的存放位置
<pre>
[root@linux-node2 ~]# cd /var/lib/cobbler/kickstarts/
</pre>
上传cfg文件，6跟7的.
<pre>
[root@linux-node2 kickstarts]# ll
total 60
-rw-r--r-- 1 root root 3704 May 28 11:59 CentOS-6-x86_64.cfg
-rw-r--r-- 1 root root 1355 May 28 11:58 CentOS-7-x86_64.cfg

</pre>

导入以后，我们查看一下
<pre>
[root@linux-node2 kickstarts]# cobbler profile report
Name                           : CentOS-7-x86_64
TFTP Boot Files                : {}
Comment                        : 
DHCP Tag                       : default
Distribution                   : CentOS-7-x86_64
Enable gPXE?                   : 0
Enable PXE Menu?               : 1
Fetchable Files                : {}
Kernel Options                 : {}
Kernel Options (Post Install)  : {}
Kickstart                      : /var/lib/cobbler/kickstarts/sample_end.ks
Name                           : CentOS-6-x86_64
TFTP Boot Files                : {}
Comment                        : 
DHCP Tag                       : default
Distribution                   : CentOS-6-x86_64
Enable gPXE?                   : 0
Enable PXE Menu?               : 1
Fetchable Files                : {}
Kernel Options                 : {}
Kernel Options (Post Install)  : {}
Kickstart                      : /var/lib/cobbler/kickstarts/sample_end.ks

</pre>

上面的2个kickstart是需要修改的，修改为我们上传的ks文件名，还有就是CentOS7的网卡信息也需要修改为eth0,操作如下：
<pre>
# cobbler profile edit --name=CentOS-7-x86_64 --kickstart=/var/lib/cobbler/kickstarts/CentOS-7-x86_64.cfg
# cobbler profile edit --name=CentOS-6-x86_64 --kickstart=/var/lib/cobbler/kickstarts/CentOS-6-x86_64.cfg
# cobbler profile edit --name=CentOS-7-x86_64 --kopts='net.ifnames=0 biosdevname=0'

</pre>

## 第五步：小试牛刀
恩，到这个地方就可以尝试来安装了，我们新开一台机器（不要告诉我你不会开机）
注意，磁盘给50G（CentOS7要求要磁盘要大点）


开机，DHCP获取正常，然后出现下面请看
![如图](http://7xwp9m.com1.z0.glb.clouddn.com/cobbler-2.png_jixuege)
很明显TFTP超时，有点需要说明的是，我服务本身状态是开启的，但是还是会提示这个错误
解决办法：
重启xinetd服务即可
<pre>
[root@linux-node2 kickstarts]# systemctl restart xinetd
</pre>
遇到过一次PXE-E51 "No DHCP or DHCP Proxy Offers received" error 
重启DHCP服务即可
<pre>
systemctl restart dhcpd
</pre>
然后就进入了选择系统界面，手动选择需要的系统即可。
如下图：
默认是从本地启动
![如图](http://7xwp9m.com1.z0.glb.clouddn.com/cobbler-3.png_jixuege)


选择一个系统来进行安装，哪里不会选哪里！！

这个时候呢，系统已经安装完毕。进入登录界面，但是呢，进入系统之后我反悔了，我安装的是6的系统，现在我不想用6了，我想用7，那么这么时候如何是好啊？
别急，下面就告诉你，如何快速解决！！
![rutu](http://7xwp9m.com1.z0.glb.clouddn.com/cobbler-4.png_jixuege)
这台是我刚通过cobbler创建的机器，系统是CentOS6，我现在要把它改为7
具体操作如下：
需要注意的是：再次强调一遍，我是在新创建的机器上执行命令。

第一：安装epel源
<pre>
rpm -ivh http://mirrors.aliyun.com/epel/epel-release-latest-7.noarch.rpm
</pre>

第二：安装软件
<pre>
yum install koan -y
</pre>
出现下面报错
<pre>
[root@bogon ~]# yum install koan -y
Loaded plugins: fastestmirror, security
Setting up Install Process
base                                                                                                                       | 3.7 kB     00:00     
base/primary_db                                                                                                            | 4.7 MB     00:07     
epel/metalink                                                                                                              | 5.4 kB     00:00     
epel                                                                                                                       | 4.3 kB     00:00     
epel/primary_db                                                                                                            | 4.1 MB     00:06     
Error: xz compression not available
</pre>
原因就是我系统是6的系统，安装了7的epel源，解决办法就是卸载这个7的epel源，然后重新安装6的epel源
<pre>
# yum remove epel-release
 rpm -ivh http://mirrors.aliyun.com/epel/epel-release-latest-6.noarch.rpm
</pre>
安装服务
故障依旧，原因就是缓存没有清除。
<pre>
[root@bogon ~]# rm -rf /var/cache/yum/x86_64/6/epel/
再次执行
yum install koan -y
</pre>

查看cobbler端可用镜像
<pre>
[root@bogon ~]# koan -s 192.168.56.12 -l profiles
- looking for Cobbler at http://192.168.56.12:80/cobbler_api
CentOS-7-x86_64
CentOS-6-x86_64
</pre>
重新安装为7的系统
<pre>
 koan -r -s 192.168.56.12 -p CentOS-7-x86_64
重启系统
reboot
</pre>

## 题外篇之使用cobbler来同步openstack源

首先上地址：[http://mirrors.aliyun.com/centos/7.2.1511/cloud/x86_64/openstack-mitaka/](http://mirrors.aliyun.com/centos/7.2.1511/cloud/x86_64/openstack-mitaka/)
这个为openstack需要使用到的源，我们用cobbler来同步，这样创建的虚拟机默认就有了源，就不需要自己一个一个安装了。

执行下面操作：
<pre>
#添加repo
 cobbler repo add --name=openstack-mitaka --mirror=http://mirrors.aliyun.com/centos/7.2.1511/cloud/x86_64/openstack-mitaka/ --arch=x86_64 --breed=yum
#同步repo
 cobbler reposync
时间有点长，耐心等待
#添加repo到CentOS7的profile文件

cobbler profile edit --name=CentOS-7-x86_64 --repos="openstack-mitaka"
#修改ks.cfg文件
在结尾添加，红色字体即可。
%post
systemctl disable postfix.service
 
$yum_config_stanza
%end
#加入到定时任务，每天同步
[root@linux-node2 kickstarts]# crontab -l
#add by jixuege at 2016-5-29
0 0 * * * /usr/bin/cobbler reposync
</pre>
同步完镜像的文件在这里
<pre>
[root@linux-node2 repo_mirror]# pwd
/var/www/cobbler/repo_mirror
[root@linux-node2 repo_mirror]# ls
openstack-mitaka
[root@linux-node2 repo_mirror]# du -sh
502M	.
</pre>

然后我们新开一台虚拟机，安装以后再次查看其源，如下所示：
有了源cobbler-config.repo，这样可以直接安装openstack了
<pre>
[root@bogon yum.repos.d]# cd /etc/yum.repos.d/
[root@bogon yum.repos.d]# more  cobbler-config.repo 
[core-0]
name=core-0
baseurl=http://192.168.56.12/cobbler/ks_mirror/CentOS-7-x86_64
enabled=1
gpgcheck=0
priority=1
[openstack-mitaka]
name=openstack-mitaka
baseurl=http://192.168.56.12/cobbler/repo_mirror/openstack-mitaka
enabled=1
priority=99
gpgcheck=0
</pre>

that's all ,enjoy!!












