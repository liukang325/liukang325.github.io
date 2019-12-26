---
layout: post
title: Python包和类的基本用法
category: Python
tags: Python
keywords: 
description: 
---

### 步骤一：建立一个文件夹filePackage

在filePackage 文件夹内创建 \__init__.py 

有了 \__init__.py  ，filePackage才算是一个包，否则只是算一个普通文件夹。


在filePackage 文件夹内创建 file.py

file.py  代码如下：

``` python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

from datetime import datetime

class MyFile():

    def __init__(self, filepath):
        print('MyFile init...')
        self.filepath = filepath

    def printFilePath(self):
        print(self.filepath)

    def testReadFile(self):
    	with open(self.filepath, 'r') as f:
		    s = f.read()
		    print('open for read...')
		    print(s)

    def testWriteFile(self):
        with open('test.txt', 'w') as f:
            f.write('今天是 ')
            f.write(datetime.now().strftime('%Y-%m-%d'))
```

\__init__.py  代码如下：

```
from file import MyFile
```
**把本模块里面的 公用的类 方法 暴漏出来**

然后 外面的引用 不用找到具体的现实位置，找到包的\__init__ 就好了

----------
### 步骤二：建立main.py 和 filePackage 平级

main.py 代码如下：

```
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

from filePackage import MyFile

if __name__ == '__main__':
	a = MyFile("./filePackage/test.txt")
	a.printFilePath();
	a.testReadFile();
```

目录结构：

![这里写图片描述](http://img.blog.csdn.net/20150702112553019)

----------

### 步骤三

若 \__init__.py  里什么也不写，那么在main.py里也可以这样写：

```
import filePackage.file
if __name__ == '__main__':
	a = filePackage.file.MyFile("./filePackage/test.txt")
	a.printFilePath();

```
但不建议这样写，建议按上面的方法**将模块里的公用类暴露出来**，直接引用。