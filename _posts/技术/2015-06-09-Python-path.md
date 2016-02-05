---
layout: post
title: python环境变量的配置
category: 技术
tags: Python
keywords: 
description: 
---

每一个python程序都需要import很多包，有些系统包是不用安装的，有些第三方包是需要安装的。

在同一台电脑里的linux环境里，安装了第三方包，基本就适用于了整个系统环境。

这里可以用以下方法，将你的开发环境独立开来：

```
mkdir .virtualenv
```
建立一个隐藏文件夹，名字可以任意取，这里是为了表示是虚拟环境。

```
cd .virtualenv

virtualenv learn1
```
创建虚拟环境learn1

```
source $HOME/.virtualenv/learn1/bin/activate
```
设置当前环境为learn1

```
(learn1)➜  ~  pip install bpython
```
你在这里通过pip安装的一些应用和第三方包，都只对learn1环境下有效

```
deactivate
```
退出当前环境

这样在learn1下你安装的应用和第三方包，在退出当前环境以后，或进入其它环境，都需要重新安装。