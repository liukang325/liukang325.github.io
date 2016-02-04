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
2. 下载DevKit-mingw64-64-4.7.2-20130224-1432-sfx.exe, 进入Devkit解压的目录

```
liukang@LIUKANG-PC /D/Ruby21-x64/Devkit
$ ruby dk.rb init
[INFO] found RubyInstaller v2.1.7 at D:/Ruby21-x64

Initialization complete! Please review and modify the auto-generated
'config.yml' file to ensure it contains the root directories to all
of the installed Rubies you want enhanced by the DevKit.

liukang@LIUKANG-PC /D/Ruby21-x64/Devkit
$ ruby dk.rb install
[INFO] Updating convenience notice gem override for 'D:/Ruby21-x64'
[INFO] Installing 'D:/Ruby21-x64/lib/ruby/site_ruby/devkit.rb'

```

安装 jekyll : 

```
gem install jekyll
```
 等待jekyll安装结束后, 查看jekyll版本号, 确定是否安装成功: 

```
liukang@LIUKANG-PC /D/Ruby21-x64/Devkit
$ gem install jekyll
Temporarily enhancing PATH to include DevKit...
Building native extensions.  This could take a while...
Successfully installed yajl-ruby-1.2.1
Fetching: posix-spawn-0.3.11.gem (100%)
Building native extensions.  This could take a while...
Successfully installed posix-spawn-0.3.11
Fetching: pygments.rb-0.6.3.gem (100%)
Successfully installed pygments.rb-0.6.3
Fetching: redcarpet-3.3.3.gem (100%)
Building native extensions.  This could take a while...
Successfully installed redcarpet-3.3.3
Fetching: blankslate-2.1.2.4.gem (100%)
Successfully installed blankslate-2.1.2.4
Fetching: parslet-1.5.0.gem (100%)
Successfully installed parslet-1.5.0
Fetching: toml-0.1.2.gem (100%)
Successfully installed toml-0.1.2
Fetching: jekyll-paginate-1.1.0.gem (100%)
Successfully installed jekyll-paginate-1.1.0
Fetching: jekyll-gist-1.3.4.gem (100%)
Successfully installed jekyll-gist-1.3.4
Fetching: coffee-script-source-1.9.1.1.gem (100%)
Successfully installed coffee-script-source-1.9.1.1
Fetching: execjs-2.6.0.gem (100%)
Successfully installed execjs-2.6.0
Fetching: coffee-script-2.4.1.gem (100%)
Successfully installed coffee-script-2.4.1
Fetching: jekyll-coffeescript-1.0.1.gem (100%)
Successfully installed jekyll-coffeescript-1.0.1
Fetching: sass-3.4.19.gem (100%)
Successfully installed sass-3.4.19
Fetching: jekyll-sass-converter-1.3.0.gem (100%)
Successfully installed jekyll-sass-converter-1.3.0
Fetching: rb-fsevent-0.9.6.gem (100%)
Successfully installed rb-fsevent-0.9.6
Fetching: ffi-1.9.10-x64-mingw32.gem (100%)
Successfully installed ffi-1.9.10-x64-mingw32
Fetching: rb-inotify-0.9.5.gem (100%)
Successfully installed rb-inotify-0.9.5
Fetching: listen-3.0.3.gem (100%)
Successfully installed listen-3.0.3
Fetching: jekyll-watch-1.3.0.gem (100%)
Successfully installed jekyll-watch-1.3.0
Fetching: fast-stemmer-1.0.2.gem (100%)
Building native extensions.  This could take a while...
Successfully installed fast-stemmer-1.0.2
Fetching: classifier-reborn-2.0.3.gem (100%)
Successfully installed classifier-reborn-2.0.3
Fetching: jekyll-2.5.3.gem (100%)
Successfully installed jekyll-2.5.3
Parsing documentation for blankslate-2.1.2.4
Installing ri documentation for blankslate-2.1.2.4
Parsing documentation for classifier-reborn-2.0.3
Installing ri documentation for classifier-reborn-2.0.3
Parsing documentation for coffee-script-2.4.1
Installing ri documentation for coffee-script-2.4.1
Parsing documentation for coffee-script-source-1.9.1.1
Installing ri documentation for coffee-script-source-1.9.1.1
Parsing documentation for execjs-2.6.0
Installing ri documentation for execjs-2.6.0
Parsing documentation for fast-stemmer-1.0.2
Installing ri documentation for fast-stemmer-1.0.2
Parsing documentation for ffi-1.9.10-x64-mingw32
Installing ri documentation for ffi-1.9.10-x64-mingw32
Parsing documentation for jekyll-2.5.3
Installing ri documentation for jekyll-2.5.3
Parsing documentation for jekyll-coffeescript-1.0.1
Installing ri documentation for jekyll-coffeescript-1.0.1
Parsing documentation for jekyll-gist-1.3.4
Installing ri documentation for jekyll-gist-1.3.4
Parsing documentation for jekyll-paginate-1.1.0
Installing ri documentation for jekyll-paginate-1.1.0
Parsing documentation for jekyll-sass-converter-1.3.0
Installing ri documentation for jekyll-sass-converter-1.3.0
Parsing documentation for jekyll-watch-1.3.0
Installing ri documentation for jekyll-watch-1.3.0
Parsing documentation for listen-3.0.3
Installing ri documentation for listen-3.0.3
Parsing documentation for parslet-1.5.0
Installing ri documentation for parslet-1.5.0
Parsing documentation for posix-spawn-0.3.11
Installing ri documentation for posix-spawn-0.3.11
Parsing documentation for pygments.rb-0.6.3
Installing ri documentation for pygments.rb-0.6.3
Parsing documentation for rb-fsevent-0.9.6
Installing ri documentation for rb-fsevent-0.9.6
Parsing documentation for rb-inotify-0.9.5
Installing ri documentation for rb-inotify-0.9.5
Parsing documentation for redcarpet-3.3.3
Installing ri documentation for redcarpet-3.3.3
Parsing documentation for sass-3.4.19
Installing ri documentation for sass-3.4.19
Parsing documentation for toml-0.1.2
Installing ri documentation for toml-0.1.2
Parsing documentation for yajl-ruby-1.2.1
Installing ri documentation for yajl-ruby-1.2.1
Done installing documentation for blankslate, classifier-reborn, coffee-script, coffee-script-source, execjs, fast-stemmer, ffi, jekyll, jekyll-coffeescript, jekyll-gist, jekyll-paginate, jekyll-sass-converter, jekyll-watch, listen, parslet, posix-spawn, pygments.rb, rb-fsevent, rb-inotify, redcarpet, sass, toml, yajl-ruby after 52 seconds
23 gems installed

liukang@LIUKANG-PC /D/Ruby21-x64/Devkit
$ jekyll -version
jekyll 2.5.3
```
4. 进到jekyll工程目录中, 启动本地服务: 

```
liukang@LIUKANG-PC /F/GitHub/liukang325.github.io (master)
$ jekyll server
Configuration file: f:/GitHub/liukang325.github.io/_config.yml
            Source: f:/GitHub/liukang325.github.io
       Destination: f:/GitHub/liukang325.github.io/_site
      Generating...
                    done.
  Please add the following to your Gemfile to avoid polling for changes:
    gem 'wdm', '>= 0.1.0' if Gem.win_platform?
 Auto-regeneration: enabled for 'f:/GitHub/liukang325.github.io'
Configuration file: f:/GitHub/liukang325.github.io/_config.yml
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
```
成功就会提示 可以访问 http://127.0.0.1:4000



###工具
【1】[RubyInstaller下载地址](https://http://rubyinstaller.org/downloads/)

【2】[MarkDown格式免费在线编辑](http://maxiang.info/)








