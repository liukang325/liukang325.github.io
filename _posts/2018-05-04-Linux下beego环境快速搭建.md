---
layout: post
title: Linux下beego环境快速搭建
category: go
tags: go Linux
keywords: 
description: 
---


1. 先安装golang环境

```
//创建目录，将作为GOPATH，为以后的代码工作目录
mkdir -p ~/goPro/bin
mkdir -p ~/goPro/src

//安装golang
$ sudo apt-get install golang

//修改~/.profile 配置环境变量
$ vi ~/.profile

//在底部增加以下内容
export GOROOT=/usr/lib/go
export PATH="$PATH:$GOROOT/bin"
export GOPATH=$HOME/goPro
export PATH="$PATH:$GOPATH/bin"

//刷新环境变量
$ source ~/.profile

//验证go版本
$ go version
go version go1.2.1 linux/386
```

2. 下载安装beego

```
$ go get github.com/astaxie/beego
$ go get github.com/beego/bee 
```

这样代码就下载到~/goPro/src/github.com/ 下面了
而且~/goPro/bin/下面多了一个bee

```
liukang@ubuntu:~$ cd ~/goPro/src 
liukang@ubuntu:~/goPro/src$ bee new hello 
liukang@ubuntu:~/goPro/src$ cd hello 
liukang@ubuntu:~/goPro/src/hello$ bee run hello
______
| ___ \
| |_/ /  ___   ___
| ___ \ / _ \ / _ \
| |_/ /|  __/|  __/
\____/  \___| \___| v1.9.1
2018/05/04 23:03:57 INFO     ▶ 0001 Using 'hello' as 'appname'
2018/05/04 23:03:57 INFO     ▶ 0002 Initializing watcher...
hello/controllers
hello/routers
hello

2018/05/04 23:04:05 SUCCESS  ▶ 0003 Built Successfully!
2018/05/04 23:04:05 INFO     ▶ 0004 Restarting 'hello'...
2018/05/04 23:04:05 SUCCESS  ▶ 0005 './hello' is running...
2018/05/04 23:04:05.755 [I] [asm_386.s:1585] http server Running on http://:8080
```

至此，beego的开发环境就搭好了。