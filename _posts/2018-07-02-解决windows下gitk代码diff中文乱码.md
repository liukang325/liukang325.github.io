---
layout: post
title: 解决windows下gitk代码diff中文乱码
category: Git
tags: Git
keywords: 
description: 
---

此博客均属原创或译文，欢迎转载但**请注明出处** 
**GithubPage:**[https://liukang325.github.io](https://liukang325.github.io)

修改gitconf ( C:\Program Files\Git\mingw64\etc\gitconfig ) 
在末尾加上如下配置：

```
[gui]
    encoding = utf-8
[i18n]
    commitencoding = utf-8
```