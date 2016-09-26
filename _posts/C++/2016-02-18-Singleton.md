---
layout: post
title: 简单的单身模式(Singleton)
category: C＋＋
tags: 设计模式
keywords: 
description: 
---

单身模式是比较常用的一种设计模式，比较简单的实现：

```
static HttpConnect * g_HttpConnect = 0;

HttpConnect * HttpConnect::getHttpSingler()
{
    if (g_HttpConnect == 0)
        g_HttpConnect = new HttpConnect();
    return g_HttpConnect;
}
```

在头文件的类中定义静态函数：

```
static HttpConnect * getHttpSingler();
```

外部调用：

```
HttpConnect::getHttpSingler()->setServerUrl("http://127.0.0.1:5000");
```


----------

**第二种方式：用模版**

```
#ifndef SINGLETON_H
#define SINGLETON_H

#include <assert.h>

template <class T>
class Singleton
{
public:
    static T* Instance()
    {
        if(!m_Instance) m_Instance = new T;
        assert(m_Instance != NULL);
        return m_Instance;
    }
protected:
    Singleton();
    ~Singleton();
private:
    Singleton(Singleton const&);
    Singleton& operator=(Singleton const&);
    static T* m_Instance;
};

template <class T> T* Singleton<T>::m_Instance=NULL;

#endif // SINGLETON_H
```

在HttpConnect 类头文件中申明：

```
typedef Singleton<HttpConnect> HttpSingle;
```

外部调用：

```
HttpSingle::Instance()->.....
```

