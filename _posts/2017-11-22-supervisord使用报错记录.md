---
layout: post
title: 在使用supervisorctl 的时候，提示上面错误refused connection
date: 2017-11-22
categories: blog
tags: [Linux]
description: 在使用supervisorctl 的时候，提示上面错误refused connection
---


## 前言
在使用supervisorctl 的时候，提示上面错误supervisorctl unix:///var/run/supervisor.sock refused connection

## 具体介绍及操作

排查半天，后来居然是机器问题。莫名其妙
以后，还是得去多思考会不会是当前环境的问题，这次就遇到了。

具体修改文件如下：

<pre>
; supervisor config file

[unix_http_server]
file=/var/run/supervisor.sock   ; (the path to the socket file)
chmod=0700                       ; sockef file mode (default 0700)

;[inet_http_server]
;port = *:9001

[supervisord]
nodaemon=true
logfile=/var/log/supervisor/supervisord.log ; (main log file;default $CWD/supervisord.log)
pidfile=/var/run/supervisord.pid ; (supervisord pidfile;default supervisord.pid)
childlogdir=/var/log/supervisor            ; ('AUTO' child log dir, default $TEMP)

; the below section must remain in the config file for RPC
; (supervisorctl/web interface) to work, additional interfaces may be
; added by defining them in separate rpcinterface: sections
[rpcinterface:supervisor]
supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface

[supervisorctl]
serverurl=unix:///var/run//supervisor.sock ; use a unix:// URL  for a unix socket
;serverurl=http://127.0.0.1:9001

; The [include] section can just contain the "files" setting.  This
; setting can list multiple files (separated by whitespace or
; newlines).  It can also contain wildcards.  The filenames are
; interpreted as relative to this file.  Included files *cannot*
; include files themselves.

[include]
files = /etc/supervisor/conf.d/*.conf

</pre>
over



