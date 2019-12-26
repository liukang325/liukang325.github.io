---
layout: post
title: QByteArray中的中文(GBK/UTF8)转成unicde(中文乱码处理)
category: Qt
tags: Qt
keywords: 
description: 
---

此博客均属原创或译文，欢迎转载但**请注明出处** 
**GithubPage:**[https://liukang325.github.io](https://liukang325.github.io)


从文件里读入一段文字到QByteArray，有的文字中文是GBK的，转成QString

```c++
text = QTextCodec::codecForName("GBK")->toUnicode(ba);
```

有的文字中文是UTF8的，转成QString

```c++
text = QTextCodec::codecForName("UTF-8")->toUnicode(ba);
```

但有时你又无法得知QByteArray中的文字编码到底是GBK还是UTF-8，所以不知道用哪个

用下面的函数就好：

```c++
#include <QTextCodec>
QString GetCorrectUnicode(const QByteArray &ba)
{
	QTextCodec::ConverterState state;
	QTextCodec *codec = QTextCodec::codecForName("UTF-8");
	QString text = codec->toUnicode(ba.constData(), ba.size(), &state);
	if (state.invalidChars > 0)
	{
		text = QTextCodec::codecForName("GBK")->toUnicode(ba);
	}
	else
	{
		text = ba;
	}
	return text;
}
```