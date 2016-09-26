---
layout: post
title: Qt android 中的Jni用法
category: QT
tags: Qt Android
keywords: 
description: 
---

先看JAVA端的代码:

```
package an.qt.useJar;
import java.lang.String;

public class ExtendsQtNative
{
    public native void sendByte(int len, byte[] date);
    public native byte[] getByteArray();
    public native void setDirectBuffer(Object buffer, int len);
}

```

JniNative.h

```
#include <QApplication>
#include <QAndroidJniEnvironment>
#include <QAndroidJniObject>
#include <jni.h>
#include "simpleCustomEvent.h"
#include <QDebug>

#define QDBG qDebug()<<__FILE__<<__FUNCTION__<<"():"<<__LINE__

#define MAX_BUF_SIZE 1024*1024*2

class JniNative
{
public:
    JniNative();
    ~JniNative();
    bool registerNativeMethods();

private:

};
```

JniNative.cpp

```
#include "JniNative.h"

extern QObject *g_listener;
uchar array[MAX_BUF_SIZE];

//javah -jni ExtendsQtNative
//javap -s -p ExtendsQtNative
static void sendByte(JNIEnv *env, jobject thiz, int len, jbyteArray date)
{
    memset(array, 0, sizeof(array));
    env->GetByteArrayRegion(date,0,len,(jbyte*)array);

    QCoreApplication::postEvent(g_listener, new SimpleCustomEvent(len, array));
}

uchar getArray[MAX_BUF_SIZE];
static jbyteArray getByteArray(JNIEnv *env, jobject thiz)
{
//    memset(getArray, 0, sizeof(getArray));
    getArray[0] = 'l';
    getArray[1] = 'k';
    getArray[2] = 'q';

    jbyteArray byteArray = env->NewByteArray(MAX_BUF_SIZE);
    env->SetByteArrayRegion(byteArray,0,MAX_BUF_SIZE,(jbyte*)getArray);

    return byteArray;
}
uchar * gBuffer;
static void setDirectBuffer(JNIEnv *env, jobject thiz, jobject buffer, jint len)
{
    //无需拷贝，直接获取与Java端共享的直接内存地址(效率比较高，但object的构造析构开销大，建议长期使用的大型buffer采用这种方式)
    gBuffer = (unsigned char *)env->GetDirectBufferAddress(buffer);
    if( gBuffer == NULL ) {
        QDBG<<"GetDirectBufferAddress Failed!";
        return;
    }
    int dataLen = len;

    //可以通过pBuffer指针来访问这段数组的值了,注意，修改数组的值后，Java端同时变化

}

JniNative::JniNative()
{
    SimpleCustomEvent::eventType();
}

JniNative::~JniNative()
{

}

bool JniNative::registerNativeMethods()
{
    JNINativeMethod methods[] {
        {"sendByte", "(I[B)V", (void*)sendByte},
        {"getByteArray", "()[B", (void*)getByteArray},
        {"setDirectBuffer","(Ljava/lang/Object;I)V",(void*)setDirectBuffer}
    };

    const char *classname = "an/qt/useJar/ExtendsQtNative";
    jclass clazz;
    QAndroidJniEnvironment env;

    QAndroidJniObject javaClass(classname);
    clazz = env->GetObjectClass(javaClass.object<jobject>());
    QDBG << "find ExtendsQtNative - " << clazz;
    bool result = false;
    if(clazz)
    {
        jint ret = env->RegisterNatives(clazz, methods, sizeof(methods) / sizeof(methods[0]));
        env->DeleteLocalRef(clazz);
        QDBG << "RegisterNatives return - " << ret;
        result = ret >= 0;
    }
    if(env->ExceptionCheck())
        env->ExceptionClear();
    return result;
}
```

主函数中需要先将这些Jni层进行注册:

main.cpp

```
{
	...

	JniNative jNative;
	jNative.registerNativeMethods();
	
	...

}

```

-------------
C++进行回调JAVA的函数:

ExtendsQtWithJava.java

```
public class ExtendsQtWithJava extends org.qtproject.qt5.android.bindings.QtActivity
{
    private final static int BUFFER_SIZE = 1024*1024*2;
    private static ByteBuffer mDirectBuffer;
    private static int dataLen;
    public ExtendsQtWithJava(){
        mDirectBuffer = ByteBuffer.allocateDirect(BUFFER_SIZE);
    }

    private final static int HEAD_OFFSET = 512;

    private final static String TAG = "~~~~~~~~~~~~~~~~~~~~~extendsQt";

    public static ExtendsQtNative m_nativeNotify = null;
    private static void notifyQt(int len, byte[] data){
        if(m_nativeNotify == null){
            m_nativeNotify = new ExtendsQtNative();
        }
        m_nativeNotify.sendByte(len,data);
    }
    private static byte[] getByteArrayFromQt(){
        if(m_nativeNotify == null){
            m_nativeNotify = new ExtendsQtNative();
        }
        return m_nativeNotify.getByteArray();
    }
    private static void setDirectBuffer()
    {
        if(m_nativeNotify == null){
            m_nativeNotify = new ExtendsQtNative();
        }
        m_nativeNotify.setDirectBuffer(mDirectBuffer,BUFFER_SIZE);
    }

    public static void start() {
        setDirectBuffer();
    }

    public static void stop() {
        Log.e(TAG, "buf0==p=" +mDirectBuffer.array()[0]);
        Log.e(TAG, "buf1==p=" +mDirectBuffer.array()[1]);
        Log.e(TAG, "buf2==p=" +mDirectBuffer.array()[2]);
    }
    public static void flsh(int len) {
        dataLen = len;
        Log.e(TAG, "buf0==p=" +mDirectBuffer.array()[0]);
        Log.e(TAG, "buf1==p=" +mDirectBuffer.array()[1]);
        Log.e(TAG, "buf2==p=" +mDirectBuffer.array()[2]);
        Log.e(TAG, "len==========" +dataLen);
    }

```

Widget.cpp

```
void Widget::on_pushButton_clicked()
{
#ifdef WIN32
    m_resultView->setText("Sorry, Just for Android!");
#elif defined(ANDROID)
//    QString url = m_urlEdit->text();
//    QAndroidJniObject javaAction = QAndroidJniObject::fromString(url);
    QAndroidJniObject::callStaticMethod<void>("an/qt/useJar/ExtendsQtWithJava",
                                       "start",
                                       "()V");
//                                       javaAction.object<jstring>());
#endif
}

uchar buf1[8] = "abc";
uchar buf2[8] = "lkq";
void Widget::on_pushButton_stop_clicked()
{
    memcpy(gBuffer, buf1, sizeof(buf1));
    QAndroidJniObject::callStaticMethod<void>(
                "an/qt/useJar/ExtendsQtWithJava",
                "stop",
                "()V");
}

void Widget::on_pushButton_flush_clicked()
{
    memcpy(gBuffer, buf2, sizeof(buf2));
    static int len = 2;
    len++;
    QAndroidJniObject::callStaticMethod<void>(
                "an/qt/useJar/ExtendsQtWithJava",
                "flsh",
                "(I)V",
                len);
}
```