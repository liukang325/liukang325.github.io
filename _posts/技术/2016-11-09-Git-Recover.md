---
layout: post
title: Git灾难恢复
category: 技术
tags: Git
keywords: 
description: 
---


**Recover a dropped stash in Git**

1) Show all the commits at the tips of your commit graph which are no longer referenced from any branch or tag – every lost commit, including every stash commit you’ve ever created, will be somewhere in that graph. 
**git fsck –no-reflog **
2) 
**git show **
3) 
**git branch recovered-branch**

![1](http://img.blog.csdn.net/20161108131915186)
