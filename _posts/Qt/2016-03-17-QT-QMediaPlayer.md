---
layout: post
title: QMediaPlayer视频播放器
category: QT
tags: Qt
keywords: 
description: 
---

.pro 工程文件中记得加上:

```
QT       +=  multimedia multimediawidgets
```

包含头文件:

```
#include <QMediaPlayer>
#include <QVideoWidget>
```

```
    QWidget *widget = new QWidget;
    widget->resize(400, 300);   //

    QVBoxLayout *layout = new QVBoxLayout;
    QMediaPlayer* player = new QMediaPlayer;
    QVideoWidget* vw = new QVideoWidget;

    layout->addWidget(vw);
    widget->setLayout(layout);
    player->setVideoOutput(vw);

//    QFile file("D://1.mp4");
//    if(!file.open(QIODevice::ReadOnly))
//        qDebug() << "Could not open file";

    player->setMedia(QUrl::fromLocalFile("D://1.mp4"));
    player->play();
    widget->show();
```

运行的时候会报这个错:

```
DirectShowPlayerService::doRender: Unresolved error code 80040266
```
原因是因为Qt 在windows上的多媒体播放功能是使用系统的DirectShow，所以安装或者更新DirectShow解码器就行了。
软件名: LAVFilters-0.65.exe

安装后就可完美运行视频播放了.