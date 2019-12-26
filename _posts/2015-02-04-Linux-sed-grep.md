---
layout: post
title: linux下用sed和grep命令替换
category: Linux
tags: Linux
keywords: 
description: 
---

linux下用sed和grep命令替换目录下所有文件中的字符串

试例如下：

```
sed -i 's/arm-none-linux-gnueabi-g++/arm-linux-g++\ -O2\ -I\$\(INC\)\ -I\$\(INCSYS\)\ -static/g' `grep arm-none-linux-gnueabi-g++ . -rl`
sed -i 's/arm-none-linux-gnueabi-gcc/arm-linux-gcc\ -static/g' `grep arm-none-linux-gnueabi-gcc . -rl`
sed -i 's/arm-none-linux-gnueabi/arm-linux/g' `grep arm-none-linux-gnueabi . -rl`
sed -i 's/nand1-2/mtd/g' `grep nand1-2 . -rl`
sed -i 's/\/opt\/RTU\/data\/etc/\/mnt\/mtd/g' `grep /opt/RTU/data/etc . -rl`
```
第三条命令是将当前目录下代码中所有的"arm-none-linux-gnueabi" 替换成 "arm-linux"