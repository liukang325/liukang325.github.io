---
layout: post
title: Qt打印调试信息输出到android logcat中
category: QT
tags: Qt Android
keywords: 
description: 
---

将Qt调试信息输出到android logcat中:

```
#include <stdarg.h>
#ifdef ANDROID
#include <android/log.h>
#define  LOGI(...)  __android_log_print(ANDROID_LOG_INFO,"SDL",__VA_ARGS__)
#define  LOGD(...)  __android_log_print(ANDROID_LOG_DEBUG,"SDL",__VA_ARGS__)
#define  LOGE(...)  __android_log_print(ANDROID_LOG_ERROR,"SDL",__VA_ARGS__)

#else
#define  LOGI(...)  {printf(__VA_ARGS__);printf("\n");fflush(stdout);}while(0)
#define  LOGD(...)  {printf(__VA_ARGS__);printf("\n");fflush(stdout);}while(0)
#define  LOGE(...)  {printf(__VA_ARGS__);printf("\n");fflush(stderr);}while(0)
#endif
```

qDebug也是可以输出的:

```
#include <QDebug>
#define QDBG qDebug()<<__FILE__<<__FUNCTION__<<"():"<<__LINE__  
```