---
layout: post
title: 命令行编译obs-studio带cef编译
category: C++
tags: OBS
keywords: 
description: 
---

此博客均属原创或译文，欢迎转载但**请注明出处** 
**GithubPage:**[https://liukang325.github.io](https://liukang325.github.io)

早期写过一篇关于obs-studio编译的博客 [obs-studio的编译环境配置](http://liukang325.github.io/2017/02/16/obs-studio的编译环境配置)

图文应该比较清楚了，但每次配一次也需要花不少时间，主要是用鼠标选择很多环境变量。现在用cmake命令行的方式整理下。

其它细节就请参考另一篇博客。

## 1. 编译cef_binary_3.3112.1659.gfef43e0_windows32.tar.bz2

创建D:\BaiduNetdiskDownload\cef_binary_3.3112.1659.gfef43e0_windows32\build

打开MSBuild Command Prompt for VS2015

```
cmake -G "Visual Studio 14" ..\

msbuild cef.sln /p:Configuration=Release /t:libcef_dll_wrapper
```
(cef.sln中包含一些例子，编译会有问题也不需要。这里只需要libcef_dll_wrapper.lib)


## 2. 编译obs-studio

```
cmake -DDepsPath=D:\BaiduNetdiskDownload\dependencies2015\win32 -DQTDIR=D:\Qt\Qt5.7.1\5.7\msvc2015 -DCEF_ROOT_DIR=D:\BaiduNetdiskDownload\cef_binary_3.3112.1659.gfef43e0_windows32 -DCEFWRAPPER_LIBRARY=D:\BaiduNetdiskDownload\cef_binary_3.3112.1659.gfef43e0_windows32\build\libcef_dll_wrapper\Release\libcef_dll_wrapper.lib -G "Visual Studio 14" ..\obs-studio

msbuild obs-studio.sln /p:Configuration=Release
```
