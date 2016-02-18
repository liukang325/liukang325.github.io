---
layout: post
title: Qt中QString,char,int,QByteArray之间的转换
category: QT
tags: QT
keywords: 
description: 
---

QT中的数据格式QString 与 QByteArray ，有些API中的参数只能是其中的某一种。

每每遇到这些，都要查找这些数据类型如何转换，这是以前在网上找到。转载过来，记录笔记方便以后查阅！

各种数据类型的相互转换

char* 与 const char *的转换

```
char*ch1="hello11";
const char *ch2="hello22";
ch2= ch1;//不报错，但有警告
ch1= (char *)ch2;
```

char 转换为QString：

```
chara='b';
QString str;
str=QString(a);
```

QString 转换为char

```
QStringstr="abc";
char *ch;
ch =str.toLatin1.data();
```

QByteArray 转换为char *

```
char*ch;//不要定义成ch[n];
QByteArraybyte;
ch = byte.data();
```

char * 转换为QByteArray

```
char*ch;
QByteArray byte;
byte = QByteArray(ch);
```

QString 转换为QByteArray

```
QByteArraybyte;
QString string;
byte = string.toAscii();
```

QByteArray转换为 QString

```
QByteArraybyte;
QString string;
string = QString(byte);
```

这里再对这俩中类型的输出总结一下：

```
qDebug()<<"print";
qDebug()<<tr("print");
qDebug()<<ch;(ch为char类型)
qDebug()<<tr(ch);
qDebug()<<byteArray;(byteArray是QByteArray类型)
qDebug()<<tr(byteArray);
qDebug()<<str;(str为Qstring类型)
但是qDebug()<<tr(str);是不可以的，要想用tr()函数输出QString类型的字符则要如下：
qDebug()<<tr(str.toLatin1);
```

int转 QString

```
inta=10;
QString b;
b=QString::number(a)
```

QString转int

```
QString a="120"
int b;
b=a.toInt（）
```

QString转float

```
QStringstr = "23.5"
float f;
f= str.toFloat();
```

float转QString

```
float f;
QString str =QString("float is %1").arg(f);
```