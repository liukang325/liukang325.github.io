---
layout: post
title: Windows下搭建本地jakyll调试环境
category: 技术
tags: jekyll
keywords: 
description: 
---

markdown写完后不是立即可见，所以每次写完文章都要经过在线调试，而在线调试就得上线文章，每次上线都得重复git add， git commit， git push这三步。非常的繁琐。

下面介绍在Windows环境下搭建本地jakyll服务器:

###步骤：
1. 下载RubyInstaller, 安装开始的时候，有三个多选按钮，都勾上, path等环境变量都会自动加到系统
2. 安装 jekyll : 
```
gem install jekyll
```
3. 等待jekyll安装结束后, 查看jekyll版本号, 确定是否安装成功: 
```
jekyll -version
```
4. 进到jekyll工程目录中, 启动本地服务: 
```
jekyll server
```
5. 成功就会提示 可以访问 http://127.0.0.1:4000



###工具
【1】[RubyInstaller下载地址](https://http://rubyinstaller.org/downloads/)








