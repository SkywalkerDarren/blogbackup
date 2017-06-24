---
title: 科学上网教程
date: 2017-06-23 18:02:25
tags: [shadowsocks, hosts, 科学上网]
---
这两天被微博刷屏了，自从习大大上台后，网络媒体的自由度一步步缩紧，媒体都要姓党，网络封锁越来越严重。对于作为严重依赖谷歌搜索的我来说内心很难过。后来政策再一次紧缩，GFW变厚，以封锁违规VPN的名义开始了vpn大屠杀，最终到了17年6月份著名的GREEN加速器终于还是顶不住了宣布了关闭服务的公告。作为续费了好几年的老用户，我对此表示深感遗憾的同时对于我来说无疑也是巨大的打击。这也迫使我寻求新的解决方案以突破GFW的封锁。以下我会介绍几种方法供大家选择。

![](http://okqqc0v2n.bkt.clouddn.com/17-6-23/55361165.jpg)
<!--more-->

# shadowsocks

shadowsocks是一种基于Socks5代理方式的网络数据加密传输包。原作者Clowwindy称受到了中国政府的压力，宣布停止维护此计划（项目）并移除其个人页面所存储的源代码。因为移除之前就有大量的复制副本，所以事实上并未停止维护，而是转由其他贡献者们持续维护中。

各个版本的[客户端地址](https://shadowsocks.org/en/download/clients.html)，这里仅演示Windows和Ubuntu的使用教程。

## Windows

首先，从这里下载[windows客户端](https://github.com/shadowsocks/shadowsocks-windows/releases)，解压得到客户端，点击打开可以看到这样的画面

![](http://okqqc0v2n.bkt.clouddn.com/17-6-23/25103642.jpg)

然后分别填入**服务器地址**、**服务器端口**、**密码**、**加密方式**，这四个重要信息就可以正常科学上网了。

至于账号密码的获得，网上有大量的免费的或收费的账号可供你选择，[这里](https://github.com/Alvin9999/new-pac/wiki/ss%E5%85%8D%E8%B4%B9%E8%B4%A6%E5%8F%B7)也有大量的免费账号。

## Linux

linux