---
layout: post
title: 重构qDebug()<<使log输出到文件
category: Qt
tags: Qt
keywords: 
description: 
---

重构qDebug()<<，使log输出到文件

```c++
#include <QProcessEnvironment>
#include <QDateTime>
#include <QFile>
#include <QIODevice>

class HSDbg
{
public:

    HSDbg& operator<<(const QString& str)
    {
        QTextStream txtOutput(&qtLogfile);
        if(qtLogfile.open(QIODevice::Append | QIODevice::Text))
        {
            qDebug() << str;
            txtOutput << str << "\n";
            qtLogfile.close();
        }
        return *this;
    }
    HSDbg& operator<<(const QByteArray& str)
    {
        QTextStream txtOutput(&qtLogfile);
        if(qtLogfile.open(QIODevice::Append | QIODevice::Text))
        {
            qDebug() << str;
            txtOutput << str << "\n";
            qtLogfile.close();
        }
        return *this;
    }
    HSDbg& operator<<(const char* str)
    {
        QTextStream txtOutput(&qtLogfile);
        if(qtLogfile.open(QIODevice::Append | QIODevice::Text))
        {
            qDebug() << QString(str);
            txtOutput << str << "\n";
            qtLogfile.close();
        }
        return *this;
    }
    HSDbg& operator<<(const int& i)
    {
        QTextStream txtOutput(&qtLogfile);
        if(qtLogfile.open(QIODevice::Append | QIODevice::Text))
        {
            qDebug() << i;
            txtOutput << i << "\n";
            qtLogfile.close();
        }
        return *this;
    }
    HSDbg(QString fileName)
    {
        qtLogfile.setFileName(fileName);
    }

private:
    QFile qtLogfile;

};

#define LOG_FILE_PATH   QProcessEnvironment::systemEnvironment().value("APPDATA") + "\\test\\logs\\"
#define LOG_FILE_NAME   "Qt_" + QDateTime::currentDateTime().toString("yyyy-MM-dd_hh-mm-ss") + ".txt"

static HSDbg sHSDbg(LOG_FILE_PATH + LOG_FILE_NAME);
//用此QDBG输出到log文件中
#define QDBG sHSDbg<<"==="+QString(__FILE__)+" "+QString(__FUNCTION__)+"():"+QString::number(__LINE__)
//用此QDBG输出到consloe上
//#define QDBG qDebug()<<__FILE__<<__FUNCTION__<<"():"<<__LINE__
```