---
layout: post
title: QNX openGL ES 图形界面环境配置
category: QNX
tags: QNX
keywords: 
description: 
---

**配置qnx6.6 VM 中 screen 环境变量**
-----------------------------
根据 Screen Graphics Subsystem Developer's Guide 总结。


1. 停止 screen 进程： slay screen
2. 确保 screen 进程已停止： pidin ar 查看当前运行进程
3. 设置环境变量：GRAPHICS_ROOT 

```
export GRAPHICS_ROOT=/usr/lib/graphics/vmware
``` 
设置环境变量：LD_LIBRARY_PATH

```
export LD_LIBRARY_PATH=/usr/lib:/lib:/lib/dll:$GRAPHICS_ROOT:$LD_LIBRARY_PATH
```

**运行 screen ： /sbin/screen** 

正常的话，屏幕会变大，全黑，鼠标出现。
不能再进行命令行操作。需要用SDP中的Terminal 连接qnx


----------


**如何用Terminal连接qnx**
--------------------

（1）在虚拟机的/etc目录中找到inetd.conf这个文件，打开，将一下四行的“#”去掉

```
	#ftp        stream tcp nowait root  /usr/sbin/ftpd           in.ftpd -l
	#telnet     stream tcp nowait root  /usr/sbin/telnetd        in.telnetd 
    #ftp        stream tcp6 nowait  root    /usr/sbin/ftpd       ftpd -ll
	#telnet     stream tcp6 nowait  root    /usr/sbin/telnetd    telnetd
```
（2）执行命令 /usr/sbin/inetd & 

（3）在qnx IDE打开Terminal：**Window**->**show view**->**other**->**Terminal**

（4）Terminal的选项卡里面，设置connection type为Telnet，之后在host输入虚拟机IP，点击确定即可。

 

----------


**测试egl**
-----
运行/usr/bin/egl-configs 出现如下信息，说明egl可以跑了

```
 # /usr/bin/egl-configs 
EGL_VENDOR = QNX Software Systems
EGL_VERSION = 1.4
EGL_CLIENT_APIS = OpenGL_ES
EGL_EXTENSIONS = EGL_EXT_swap_regions EGL_KHR_image 

+-----------+--------+-----+-------+------+-----------+-------+-------+---------+------+---------+--------+---------+--------+----+-----+-----+-------+-------+----+----+----
----+-------+-----+-------+------+
|           |        |     |       |      |           |       |       |         |      |         |        |         |        |    |     |     |       |       |    |    |    
    |       |    transparent     |
| config id | caveat | red | green | blue | luminance | alpha | depth | stencil | mask | samples | window | pbuffer | pixmap | sw | lin | pre | gles1 | gles2 | vg | gl | nat
ive | level | red | green | blue |
+-----------+--------+-----+-------+------+-----------+-------+-------+---------+------+---------+--------+---------+--------+----+-----+-----+-------+-------+----+----+----
----+-------+-----+-------+------+
|         1 |  slow  |   8 |     8 |    8 |         0 |     8 |     0 |       0 |    0 |       0 |    x   |    x    |    x   |    |     |     |   x   |       |    |    |    
    |     0 | n/a |  n/a  | n/a  |
|         2 |  slow  |   8 |     8 |    8 |         0 |     8 |     0 |      16 |    0 |       0 |    x   |    x    |    x   |    |     |     |   x   |       |    |    |    
    |     0 | n/a |  n/a  | n/a  |
|         3 |  slow  |   8 |     8 |    8 |         0 |     8 |    16 |       0 |    0 |       0 |    x   |    x    |    x   |    |     |     |   x   |       |    |    |    
    |     0 | n/a |  n/a  | n/a  |
|         4 |  slow  |   8 |     8 |    8 |         0 |     8 |    16 |      16 |    0 |       0 |    x   |    x    |    x   |    |     |     |   x   |       |    |    |    
    |     0 | n/a |  n/a  | n/a  |
|         5 |  slow  |   8 |     8 |    8 |         0 |     0 |     0 |       0 |    0 |       0 |    x   |    x    |    x   |    |     |     |   x   |       |    |    |    
    |     0 | n/a |  n/a  | n/a  |
|         6 |  slow  |   8 |     8 |    8 |         0 |     0 |     0 |      16 |    0 |       0 |    x   |    x    |    x   |    |     |     |   x   |       |    |    |    
    |     0 | n/a |  n/a  | n/a  |
|         7 |  slow  |   8 |     8 |    8 |         0 |     0 |    16 |       0 |    0 |       0 |    x   |    x    |    x   |    |     |     |   x   |       |    |    |    
    |     0 | n/a |  n/a  | n/a  |
|         8 |  slow  |   8 |     8 |    8 |         0 |     0 |    16 |      16 |    0 |       0 |    x   |    x    |    x   |    |     |     |   x   |       |    |    |    
    |     0 | n/a |  n/a  | n/a  |
|         9 |  slow  |   5 |     5 |    5 |         0 |     1 |     0 |       0 |    0 |       0 |    x   |    x    |    x   |    |     |     |   x   |       |    |    |    
    |     0 | n/a |  n/a  | n/a  |
|        10 |  slow  |   5 |     6 |    5 |         0 |     0 |     0 |       0 |    0 |       0 |    x   |    x    |    x   |    |     |     |   x   |       |    |    |    
    |     0 | n/a |  n/a  | n/a  |
|        11 |  slow  |   5 |     5 |    5 |         0 |     1 |     0 |      16 |    0 |       0 |    x   |    x    |    x   |    |     |     |   x   |       |    |    |    
    |     0 | n/a |  n/a  | n/a  |
|        12 |  slow  |   5 |     6 |    5 |         0 |     0 |     0 |      16 |    0 |       0 |    x   |    x    |    x   |    |     |     |   x   |       |    |    |    
    |     0 | n/a |  n/a  | n/a  |
|        13 |  slow  |   5 |     5 |    5 |         0 |     1 |    16 |       0 |    0 |       0 |    x   |    x    |    x   |    |     |     |   x   |       |    |    |    
    |     0 | n/a |  n/a  | n/a  |
|        14 |  slow  |   5 |     6 |    5 |         0 |     0 |    16 |       0 |    0 |       0 |    x   |    x    |    x   |    |     |     |   x   |       |    |    |    
    |     0 | n/a |  n/a  | n/a  |
|        15 |  slow  |   5 |     5 |    5 |         0 |     1 |    16 |      16 |    0 |       0 |    x   |    x    |    x   |    |     |     |   x   |       |    |    |    
    |     0 | n/a |  n/a  | n/a  |
|        16 |  slow  |   5 |     6 |    5 |         0 |     0 |    16 |      16 |    0 |       0 |    x   |    x    |    x   |    |     |     |   x   |       |    |    |    
    |     0 | n/a |  n/a  | n/a  |
+-----------+--------+-----+-------+------+-----------+-------+-------+---------+------+---------+--------+---------+--------+----+-----+-----+-------+-------+----+----+----
----+-------+-----+-------+------+
```

demo