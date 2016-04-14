---
layout: post
title: Android布局初步
category: Android
tags: android
keywords: 
description: 
---

```
android:id  -- 为控件指定相应的ID
android:text  -- 指定控件当中显示的文字, 尽量使用string.xml中的文字
android:grivity  -- 指定控件的基本位置, 比如说居中 居右等位置
android:textSize  -- 指定控件当中字体的大小
android:background  -- 指定该控件所使用的背景色,RGB命名法
android:width  -- 指定控件的宽度
android:height  -- 指定控件的高度

android:padding*  -- 指定控件的内边距, 也就是说控件当中的内容
android:paddingBottom	--以像素为单位设置下边的内边距. 
android:paddingLeft	 --	以像素为单位设置左边的内边距. 
android:paddingRight  -- 以像素为单位设置右边的内边距. 
android:paddingTop  -- 以像素为单位设置上边的内边距. 

android:sigleLine="true"  -- 如果设置为真, 则将控件的内容在同一行当中进行显示
```

Android中RelativeLayout各个属性的含义:

```
android:layout_above="@id/xxx"  --将控件置于给定ID控件之上
android:layout_below="@id/xxx"  --将控件置于给定ID控件之下
android:layout_toLeftOf="@id/xxx"  --将控件的右边缘和给定ID控件的左边缘对齐
android:layout_toRightOf="@id/xxx"  --将控件的左边缘和给定ID控件的右边缘对齐

android:layout_alignLeft="@id/xxx"  --将控件的左边缘和给定ID控件的左边缘对齐
android:layout_alignTop="@id/xxx"  --将控件的上边缘和给定ID控件的上边缘对齐
android:layout_alignRight="@id/xxx"  --将控件的右边缘和给定ID控件的右边缘对齐
android:layout_alignBottom="@id/xxx"  --将控件的底边缘和给定ID控件的底边缘对齐
android:layout_alignParentLeft="true"  --将控件的左边缘和父控件的左边缘对齐
android:layout_alignParentTop="true"  --将控件的上边缘和父控件的上边缘对齐
android:layout_alignParentRight="true"  --将控件的右边缘和父控件的右边缘对齐
android:layout_alignParentBottom="true" --将控件的底边缘和父控件的底边缘对齐

android:layout_centerInParent="true"  --将控件置于父控件的中心位置
android:layout_centerHorizontal="true"  --将控件置于水平方向的中心位置
android:layout_centerVertical="true"  --将控件置于垂直方向的中心位置
```