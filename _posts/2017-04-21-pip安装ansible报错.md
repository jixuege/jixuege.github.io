---
layout: post
title: pip 安装ansible提示 error command gcc failed with exit status 1
date: 2017-04-21
categories: blog
tags: [error,pip]
description: pip 安装ansible提示 error command gcc failed with exit status 1
---

## 前言
装ansible报错error: command 'gcc' failed with exit status 1

## 具体介绍及操作
具体错误信息如下：
<pre>
gcc -pthread -fno-strict-aliasing -O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -D_GNU_SOURCE -fPIC -fwrapv -DNDEBUG -O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -D_GNU_SOURCE -fPIC -fwrapv -fPIC -I/usr/include/python2.7 -c build/temp.linux-x86_64-2.7/_openssl.c -o build/temp.linux-x86_64-2.7/build/temp.linux-x86_64-2.7/_openssl.o
    build/temp.linux-x86_64-2.7/_openssl.c:2:20: fatal error: Python.h: No such file or directory
     #include <Python.h>
                        ^
    compilation terminated.
    error: command 'gcc' failed with exit status 1
    
    ----------------------------------------
Command "/usr/bin/python -c "import setuptools, tokenize;__file__='/tmp/pip-build-ZZA2W8/cryptography/setup.py';exec(compile(getattr(tokenize, 'open', open)(__file__).read().replace('\r\n', '\n'), __file__, 'exec'))" install --record /tmp/pip-9cc_pW-record/install-record.txt --single-version-externally-managed --compile" failed with error code 1 in /tmp/pip-build-ZZA2W8/cryptography
</pre>

解决办法如下：
<pre>
yum -y install gcc gcc-c++ kernel-devel
yum -y install python-devel libxslt-devel libffi-devel openssl-devel

</pre>


更新完毕！！enjoy！！

