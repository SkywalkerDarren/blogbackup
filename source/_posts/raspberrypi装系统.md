---
title: raspberry pi 装系统
date: 2017-02-27 20:44:58
tags: [树莓派]
---
Raspberry Pi原本是一款针对计算机编程教育的迷你计算机，仅有银行卡大小，但有丰富的外部接口，如HDMI，DSI，CSI，USB，GPIO，TF卡接口以及3.5mm音频接口。因此很多计算机爱好者开始用PI做各种各样的开发，从装了Kali的渗透利器到无所不能的家庭影院，都可以用树莓派来完成。可谓“麻雀虽小五脏俱全”。

![](http://p1.bqimg.com/567571/4abf113a486e4b4e.png)

<!--more-->

那这里就先来介绍下树莓派系统的安装方法。

# Raspberry Pi的系统选择

能适配Raspberry Pi的linux系统有很多，这里新手推荐树莓派基金会基于Debian专门为树莓派开发的Raspbian。下面也会给出一系列能适配树莓派的系统。

+ [Raspbian](https://www.raspberrypi.org/downloads/raspbian/)  推荐，可以不用显示器，只用一根网线就可以安装
+ [Noobs](https://www.raspberrypi.org/downloads/noobs/)  官方的多系统引导
+ [Ubuntu Mate](https://ubuntu-mate.org/raspberry-pi/)
+ [Snappy Ubuntu Core](https://developer.ubuntu.com/core/get-started/raspberry-pi-2-3)
+ [Kali](http://mirrors.neusoft.edu.cn/kali-images/kali-2016.2/kali-linux-light-2016.2-armhf.img.xz)
+ [Arch](http://sg.mirror.archlinuxarm.org/os/ArchLinuxARM-rpi-latest.tar.gz)
+ [Win10 IOT](https://developer.microsoft.com/zh-cn/windows/iot/Downloads.htm)

# Raspbian系统烧录

这里推荐的系统是Raspbian，简单容易配置所以适合新手。而系统烧录的方法大都大同小异，也可以借鉴，但是有的系统可能需要外接显示器，键鼠等设备才能进入系统进行配置及使用。

首先要看你的TF卡是否和树莓派兼容，[点这里](http://elinux.org/RPi_SD_cards#Working_.2F_Non-working_SD_cards)查看列表。

然后需要下载[Win32DiskDiskImager](https://sourceforge.net/projects/win32diskimager/)和Raspbian，然后解压得到一个`img`镜像。

![](http://p1.bqimg.com/567571/62b097ef9280f083.jpg)

在Win32DiskDiskImager选择你的TF卡，以及你自己的系统镜像，然后点`Write`等待烧录成功，你的系统就做好了。

# SSH直连树莓派

这里要强调一点，**最新版的raspbian默认关闭了SSH，所以先要在`boot`分区里面建立一个名为`SSH`的文件**

![](http://p1.bpimg.com/567571/b9f43cbc36627133.jpg)

首先要有一根网线，让你的树莓派与路由器相连，然后就可以在路由器的DHCP客户列表下找到你的树莓派IP了

![](http://p1.bpimg.com/567571/3024e084b254a2ae.jpg)

然后下载[PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)在里面填入对应的树莓派IP地址打开就可以了，进去后会有个安全提示，点是即可。

![](http://i1.piimg.com/567571/899c631baa064bf3.jpg)

接着输入默认的用户名密码就可以进入系统了。

```text
用户名：pi
密码：raspberry
```

![](http://p1.bpimg.com/567571/f7578549f271c84f.jpg)

然后输入下面命令开始配置系统。

```bash
sudo raspi-config
```

![](http://p1.bqimg.com/567571/d1929f4ee2adde84.jpg)

选`Advanced Options`，再选`Expand Filesystem`来扩展使用空间。重启后生效。

还有在`Interfacing Options`里面可以开启你需要的接口，比如`VNC`，`GPIO`，`I2C`，`SPI`等等。

接下来就是发挥你想象力的时候了，开始折腾你的树莓派吧。
