---
layout: post
title: Ubuntu Server连接wifi
category: Linux
tags: Linux
keywords: 
description: 
---

将旧笔记本装上ubuntu当作服务器，起初装了个 ubuntu-18.10-desktop-amd64.iso

发现没必要装桌面版，于是就装了服务版 ubuntu-18.10-live-server-amd64.iso

（使用UltraISO.exe烧写iso到U盘时，记得选择RAW模式烧写，否则无法装server版）

### 安装系统 ubuntu-18.10-live-server-amd64.iso

基本参照下面这个步骤完成

- [Ubuntu18.10最小化安装详细过程详解](http://www.piis.cn/zhishi/web1424.asp)

其它没什么要注意的，主要是中间要将国外的镜像地址 http://archive.ubuntu.com/ubuntu

换为国内的 http://mirrors.163.com/ubuntu

### 运行，连wifi

安装成功后，默认不插上网线系统是不能启动的，一直会卡在等待网络那块，无奈先找根网线插上。

由于网线有线，我需要将笔记本连上wifi。

1. 使用iwconfig命令查看无线网卡的信息, 我的已经连接上了，所以SSID也显示了，第一次配时未显示SSID

```
liukang@liukang:~$ iwconfig
wlp6s0    IEEE 802.11  ESSID:"CMCC-Qt2.4"  
          Mode:Managed  Frequency:2.462 GHz  Access Point: 3C:E8:24:2E:1E:B8   
          Bit Rate=6.5 Mb/s   Tx-Power=15 dBm   
          Retry short limit:7   RTS thr:off   Fragment thr:off
          Power Management:off
          Link Quality=45/70  Signal level=-65 dBm  
          Rx invalid nwid:0  Rx invalid crypt:0  Rx invalid frag:0
          Tx excessive retries:15  Invalid misc:422   Missed beacon:0

lo        no wireless extensions.

enp7s0    no wireless extensions.
```

能正常显示出wlp6s0，说明电脑上存在无线网卡, 有的人是wlan0

2. 启动无线网卡项，或者确认其是启动的：

```
sudo ip link set wlp6s0 up
```

3. 扫描所检测到的无线网络

```
sudo iw dev wlp6s0 scan | less
```

这一步会扫描出无线网卡能搜索到的所有无线wifi，我们需要从里面选择一个wifi进行连接。

4. 连接网络

如果所连接的网络是开放的、没有加密的，则可以轻松地直接连接：

```
ubuntu:~$ sudo iw dev wlp6s0 connect [网络 SSID]
```

如果网络是用较低级的协议，WEP加密的，则也比较容易：

```
ubuntu:~$ sudo iw dev wlp6s0 connect [网络 SSID] key 0:[WEP 密钥]
```

如果网络使用的是WPA或者WPA2协议，则需要使用一个叫做wpasupplicant的工具，通过如下命令可以自动安装：

```
ubuntu:~$ sudo apt install wpasupplicant
ubuntu:~$ sudo vim /etc/wpa_supplicant/wpa_supplicant.conf
```

```
ctrl_interface=/var/run/wpa_supplicant

ap_scan=1

network={
	ssid="CMCC-Qt2.4"
	psk="mypassword"
	priority=1
}
```

然后在后台启动
```
sudo wpa_supplicant -i wlp6s0 -c /etc/wpa_supplicant/wpa_supplicant.conf &
```

有时这个会运行失败，是因为后台已经有一个wpa_supplicant的进程在启动，需要先杀掉
```
pkill wpa_supplicant
```

5. 使用dhclient或者dhcpcd命令为本机获取ip地址：

```
sudo dhclient wlp6s0
```

6. 成功，通过ifconfig 查看wlp6s0（wlan0）的ip地址，说明连接成功，就可以拔网线了。

但如果重启好像又必须插上网线才能成功启动


=================

为了方便每次连接wifi， 在配置好 /etc/wpa_supplicant/wpa_supplicant.conf 的前提下，我写了个脚本

wlan.sh
```
sudo pkill wpa_supplicant
sudo wpa_supplicant -i wlp6s0 -c /etc/wpa_supplicant/wpa_supplicant.conf &
sudo dhclient wlp6s0
```

下次运行脚本 即可方便连接wifi