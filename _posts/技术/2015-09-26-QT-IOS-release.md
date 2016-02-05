---
layout: post
title: QT 5.2.1如何编译发布IOS程序
category: 技术
tags: QT
keywords: 
description: 
---

所用QT版本：qt-opensource-mac-x64-android-ios-5.2.1.dmg

下载地址：[http://download.qt.io/official_releases/qt/5.2/5.2.1/](http://download.qt.io/official_releases/qt/5.2/5.2.1/)

MAC系统：MAC OS X 10.8虚拟机

1. 安装QT后选择iphoneos-clang-...那个编译套件，release编译生成一个 qt_ios.app
2. qt_ios是我的项目名
3. 将qt_ios.app 放入Payload文件夹中
4. 将Payload压缩成Payload.zip包
5. 然后将Payload.zip重命名为后缀为.ipa的包，如Payload.ipa
6. Payload.ipa 就可以直接安装在IOS手机上