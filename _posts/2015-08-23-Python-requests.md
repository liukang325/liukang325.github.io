---
layout: post
title: requests模块post/get基本用法
category: Python
tags: Python
keywords: 
description: 
---

python用requests模块写post/get接口

pip install requests 安装requests模块

实例：

```
# -*- coding: utf-8 -*-
import sys
reload(sys)
sys.setdefaultencoding('utf8')
import requests
import json

def zp(r):
    try:
        print(json.dumps(json.loads(r.text), indent=4,ensure_ascii=False, encoding="utf-8"))
    except Exception, e:
        print r.text

def login():
    data = {'userName': '15827241843', 'password': '123456'}
    r = requests.post("http://localhost:5000/Login", data=data)
    zp(r)


def logout():
    r = requests.get("http://localhost:5000/Logout?sessionID=763c962e3f3011e588bdb8975a9c70dc")
    zp(r)


def RouteStation():
    url = "http://localhost:5000/RouteStation?"
    url += "fromAdress=1"
    url += "&toAddress=2"
    url += "&date=2015-08-20"
    r = requests.get(url)
    zp(r)
```