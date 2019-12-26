---
layout: post
title: How to Debug a Release Build
category: Tools
tags: C++
keywords: 
description: 
---


引自： https://msdn.microsoft.com/en-us/library/fsk896zz.aspx?f=255&MSPPError=-2147217396

For the latest documentation on Visual Studio 2017 RC, see Visual Studio 2017 RC Documentation.

You can debug a release build of an application.

## To debug a release build
Open the **Property Pages** dialog box for the project. For details, see Working with Project Properties.

Click the **C/C++** node. Set **Debug Information Format** to **C7 compatible (/Z7)** or **Program Database (/Zi)**.

Expand **Linker** and click the **General** node. Set **Enable Incremental Linking** to **No (/INCREMENTAL:NO)**.

Select the **Debugging** node. Set **Generate Debug Info** to Yes (/DEBUG).

Select the **Optimization** node. Set **References** to **/OPT:REF** and **Enable COMDAT Folding** to **/OPT:ICF**.

You can now debug your release build application. To find a problem, step through the code (or use Just-In-Time debugging) until you find where the failure occurs, and then determine the incorrect parameters or code.

If an application works in a debug build, but fails in a release build, one of the compiler optimizations may be exposing a defect in the source code. To isolate the problem, disable selected optimizations for each source code file until you locate the file and the optimization that is causing the problem. (To expedite the process, you can divide the files into two groups, disable optimization on one group, and when you find a problem in a group, continue dividing until you isolate the problem file.)

You can use /RTC to try to expose such bugs in your debug builds.

For more information, see Optimizing Your Code.

------------------------

以上设置有一个比较麻烦的就是，如果重新cmake后，新生成的sln，又不能调试了，必须重新设置一遍。在cmakelist.txt中加入下面的，cmake后生成的sln，就自动支持在Release下Debug了

```
#VS Release 下可调试
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /Zi")
SET_TARGET_PROPERTIES(BoZhuShou PROPERTIES COMPILE_FLAGS "/Od")
SET_TARGET_PROPERTIES(BoZhuShou PROPERTIES LINK_FLAGS "/DEBUG")
```