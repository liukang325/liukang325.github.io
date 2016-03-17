---
layout: post
title: QThread with movetothread
category: QT
tags: Qt
keywords: 
description: 
---

QThread的另一种用法, 不用继承QThread和重载run()函数:

```
 QThread *thread = new QThread( );
 Task    *task   = new Task();
 task->moveToThread(thread);
 connect( thread, SIGNAL(started()), task, SLOT(doWork()) );
 connect( task, SIGNAL(workFinished()), thread, SLOT(quit()) );
 //automatically delete thread and task object when work is done:
 connect( thread, SIGNAL(finished()), task, SLOT(deleteLater()) );
 connect( thread, SIGNAL(finished()), thread, SLOT(deleteLater()) );
 thread->start();
```

```

class Task : public QObject
{
Q_OBJECT
public:
    Task();
    ~Task();
public slots:
    void doWork();
signals:
    void workFinished();
};
```
connect()的第五个参数
有六种：

1. Qt::AutoConnection
2. Qt::DirectConnection
3. Qt::QueuedConnection
4. Qt::BlockingQueuedConnection
5. Qt::UniqueConnection
6. Qt::AutoCompatConnection

前两种比较相似，都是同一线程之间连接的方式，不同的是Qt::AutoConnection是系统默认的连接方式。这种方式连接的时候，槽不是马上被执行的，而是进入一个消息队列，待到何时执行就不是我们可以知道的了，当信号和槽不是同个线程，会使用第三种QT::QueueConnection的链接方式。如果信号和槽是同个线程，调用第二种Qt::DirectConnection链接方式。

第二种Qt::DirectConnection是直接连接，也就是只要信号发出直接就到槽去执行，无论槽函数所属对象在哪个线程，槽函数都在发射信号的线程内执行，一旦使用这种连接，槽将会不在线程执行！。

第三种Qt::QueuedConnection和第四种Qt::BlockingQueuedConnection是相似的，都是可以在不同进程之间进行连接的，不同的是，这里第三种是在对象的当前线程中执行,并且是按照队列顺序执行。当当前线程停止，就会等待下一次启动线程时再按队列顺序执行  ，等待QApplication::exec()或者线程的QThread::exec()才执行相应的槽，就是说：当控制权回到接受者所依附线程的事件循环时，槽函数被调用，而且槽函数在接收者所依附线程执行，使用这种连接，槽会在线程执行。

第四种Qt::BlockingQueuedConnection是（必须信号和曹在不同线程中，否则直接产生死锁）这个是完全同步队列只有槽线程执行完才会返回，否则发送线程也会等待，相当于是不同的线程可以同步起来执行。

第五种Qt::UniqueConnection跟默认工作方式相同，只是不能重复连接相同的信号和槽；因为如果重复链接就会导致一个信号发出，对应槽函数就会执行多次。

第六种Qt::AutoCompatConnection是为了连接QT4 到QT3的信号槽机制兼容方式，工作方式跟Qt::AutoConnection一样。显然这里我们应该选择第三种方式，我们不希望子线程没结束主线程还要等，我们只是希望利用这个空闲时间去干别的事情，当子线程执行完了，只要发消息给主线程就行了，到时候主线程会去响应。