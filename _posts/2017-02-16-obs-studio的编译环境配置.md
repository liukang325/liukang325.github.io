---
layout: post
title: obs-studio的编译环境配置
category: C++
tags: OBS
keywords: 
description: 
---

我这里根据自己遇到的情况总结一下windows下OBS的开发环境配置。

全平台可以参考：https://github.com/jp9000/obs-studio/wiki/Install-Instructions

提前准备环境：

**Git**

**VS 2015**

**Qt5** （我用的此版本：qt-opensource-windows-x86-msvc2015-5.7.1.exe）

**CMake 3.6.2**

以上几个最基本的，我就不提供下载地址了；

OBS在VS2015上开发所需要依赖的**库**：https://obsproject.com/downloads/dependencies2015.zip

首先从github上clone源代码： 
```
git clone --recursive https://github.com/jp9000/obs-studio.git 
```
或者
```
git clone https://github.com/jp9000/obs-studio.git 
git submodule update --init --recursive
```

因为obs-studio的源代码中包含其它一些开源库的git地址。所以需要--recursive 

一并将submodule中的Git源码全下到本地。

其中就包含libdshowcapture（视频捕获设备）、obs-browser（cef内核浏览器插件）等。

----------
（我这里编译windows 32bit的obs-studio）

将这些源代码下载成功后，接下来就是cmake了。我提供两种方法：
## **一、使用CMake 3.6.2 gui**##

注意在CMAKE中修改以下几项：

```
DepsPath = D:/Workspace/Coding/dependencies2015/win32
QTDIR = D:/Qt/Qt5.7.1/5.7/msvc2015
COPY_DEPENDENCIES = ON
```

如果你不需要编译obs-browser（cef内核浏览器插件），BUILD_BROWSER 可以不勾选。

如果需要，勾选 BUILD_BROWSER 后，还需要增加配置下面两个变量

```
CEFWRAPPER_LIBRARY = D:/Workspace/Coding/CEF/cef_binary_3.2704.1434.gec3e9ed_windows32/Release/libcef_dll_wrapper.lib
CEF_ROOT_DIR ＝ D:/Workspace/Coding/CEF/cef_binary_3.2704.1434.gec3e9ed_windows32
```
附上我的图：
![这里写图片描述](http://img.blog.csdn.net/20170216113949101?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1a2FuZzMyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


----------

那么cef_binary_3.2704.1434.gec3e9ed_windows32从哪来？

下载地址： https://cefbuilds.com/

我下载的 cef_binary_3.2704.1434.gec3e9ed_windows32.7z

解压至 D:/Workspace/Coding/CEF/cef_binary_3.2704.1434.gec3e9ed_windows32

再进行cmake， 先‘Configure’，再‘Generate’

![这里写图片描述](http://img.blog.csdn.net/20170216114516729?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1a2FuZzMyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

打开 D:/Workspace/Coding/CEF/cef_binary_3.2704.1434.gec3e9ed_windows32/build/cef.sln

build成功后就会生成D:/Workspace/Coding/CEF/
cef_binary_3.2704.1434.gec3e9ed_windows32/Release/libcef_dll_wrapper.lib

----------

下面回到正题obs-studio的cmake，按以上的步骤增加环境变量，先‘Configure’，再‘Generate’，就会配置成功，成功后，用VS2015打开D:/Workspace/Github/obs-studio-build/obs-studio.sln，build即可！


----------
## **二、使用QT 5.7.1**##
打开QT，[文件]->[打开文件或项目]，选择obs-studio源码下的CMakeLists.txt

![这里写图片描述](http://img.blog.csdn.net/20170216115518578?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1a2FuZzMyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

会自动进行cmake，会报错。所以进入下图配置一下DepsPath

DepsPath = D:/Workspace/Coding/dependencies2015/win32

![这里写图片描述](http://img.blog.csdn.net/20170216174943727?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1a2FuZzMyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

这里不用配置QTDIR，因为本身就是QT打开的工程，自带QTDIR的环境。

配置好后，再点一下“Apply Configuration Changes”

再去编译构建。

构建完成后，运行，会出错！ 因为QT默认的项目程序运行地址下没有obs32.exe

需要重新指定一下，如下图：

![这里写图片描述](http://img.blog.csdn.net/20170216120145362?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1a2FuZzMyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

再去QT编辑界面点那个绿色的三角形Run即可运行成功。

----------
如果勾选了BUILD_BROWSER，也编译成功了。但你发现在来源里添加源并没有BrowserSource这一个，为什么呢？

你需要将以下库拷贝到%obs-studio%\build\rundir\Release\obs-plugins\32bit

%CEF%\Release\libcef.dll

%CEF%\Release\natives_blob.bin

%CEF%\Release\snapshot_blob.bin

%CEF%\Resources\*.*

这样就可以正常使用CEF浏览器插件了。
