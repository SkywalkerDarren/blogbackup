---
title: Arch + VMware + UEFI + Gnome安装
date: 2017-05-28 17:10:16
tags: [Arch,虚拟机]
---
本文旨在从VMware+UEFI安装Arch以及配置Gnome桌面环境，更详细的的安装教程请参考[Arch Wiki](https://wiki.archlinux.org/index.php/Main_page_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))。
![](http://okqqc0v2n.bkt.clouddn.com/17-5-28/83580649.jpg)

<!--more-->

# VMware配置

首先新建虚拟机，选自定义

![](http://okqqc0v2n.bkt.clouddn.com/17-5-28/87421627.jpg)

兼容性默认即可

![](http://okqqc0v2n.bkt.clouddn.com/17-5-28/6931808.jpg)

打开你Arch所在位置

![](http://okqqc0v2n.bkt.clouddn.com/17-5-28/5418077.jpg)

客户机选Linux，内核没有Arch，随便选个64位的

![](http://okqqc0v2n.bkt.clouddn.com/17-5-28/31612588.jpg)

命名你的系统

![](http://okqqc0v2n.bkt.clouddn.com/17-5-28/74588172.jpg)

这里根据你的物理机实际情况设置

![](http://okqqc0v2n.bkt.clouddn.com/17-5-28/61965756.jpg)

![](http://okqqc0v2n.bkt.clouddn.com/17-5-28/40245215.jpg)

余下如图

![](http://okqqc0v2n.bkt.clouddn.com/17-5-28/54547146.jpg)

![](http://okqqc0v2n.bkt.clouddn.com/17-5-28/11626110.jpg)

![](http://okqqc0v2n.bkt.clouddn.com/17-5-28/62202143.jpg)

![](http://okqqc0v2n.bkt.clouddn.com/17-5-28/58882437.jpg)

根据你的实际情况分配磁盘大小和路径

![](http://okqqc0v2n.bkt.clouddn.com/17-5-28/30622448.jpg)

![](http://okqqc0v2n.bkt.clouddn.com/17-5-28/9828695.jpg)

完成
![](http://okqqc0v2n.bkt.clouddn.com/17-5-28/27575624.jpg)

看这里！！**重点！！ 设置UEFI启动**

在`虚拟机 -> 设置`的`选项`选项卡里面，选`高级`，将`通过EFI而非BIOS引导`勾选

![](http://okqqc0v2n.bkt.clouddn.com/17-5-28/73509197.jpg)

这样一来就算是设置完VMware的UEFI启动了。

# 安装前的准备

打开虚拟机选择第一个

![](http://okqqc0v2n.bkt.clouddn.com/17-5-28/38472916.jpg)

进入系统

![](http://okqqc0v2n.bkt.clouddn.com/17-5-28/23932737.jpg)

## 验证启动模式

```bash
ls /sys/firmware/efi/efivars
```

如果找不到文件那么你不是UEFI启动

请看上边最后一条的虚拟机设置UEFI启动

## 联网测试

```bash
ping -c 3 www.baidu.com
```

如果未成功请检查虚拟机的设置，如NAT服务是否打开，重启虚拟机

## 更新系统时间

```bash
timedatectl set-ntp true
```

可以用`timedatectl status`来查看当前时间（格林尼治时间）

## 建立分区

熟悉的朋友可以通过`fdisk`来分区

简单的也可以用简单的`cfdisk`的图形画界面来分区

因为我们是UEFI虚拟机，因此至少需要3个分区，EFI分区（分配大约200-300M），swap分区（在拥有不足 512 MB 内存的机器上，通常为 swap 分区分配2倍内存大小的空间。如果有更大的内存（大于 1024 MB），可以分配较少的空间，wiki建议虚拟机应该分配swap以便休眠），根分区`/`（余下空间的大小）

执行cfdisk

```bash
cfdisk /dev/sda
```

选第一个GPT

![](http://okqqc0v2n.bkt.clouddn.com/17-5-28/72579941.jpg)

选NEW分出3个分区

![](http://okqqc0v2n.bkt.clouddn.com/17-5-28/64373372.jpg)

再依次更改type

![](http://okqqc0v2n.bkt.clouddn.com/17-5-28/77816958.jpg)

最后Write保存，输入`yes`

![](http://okqqc0v2n.bkt.clouddn.com/17-5-28/5066611.jpg)

用`fdisk -l /dev/sda`来检查分区是否正确

## 格式化分区

将EFI分区格式化为FAT32

```bash
mkfs.fat -F32 /dev/sda1
```

将根分区格式化为ext4

```bash
mkfs.ext4 /dev/sda2
```

创建swap分区

```bash
mkswap /dev/sda3
swapon /dev/sda3
```

## 挂载分区

首先挂载根分区

```bash
mount /dev/sda2 /mnt
```

然后创建`boot`分区

```bash
mkdir /mnt/boot
```

在挂载efi分区到boot

```bash
mount /dev/sda1 /mnt/boot
```

如果挂错分区则用`umount`卸载分区，再重新挂载

# 安装

## 选择合适的源

编辑源

```bash
nano /etc/pacman.d/mirrorlist
```

在第一行加上163的源

```bash
Server = http://mirrors.aliyun.com/archlinux/$repo/os/$arch
```

然后更新源

```bash
pacman -Syy
```

## 安装基本系统

```bash
pacstrap /mnt base base-devel
```

# 配置系统

## 生成fstab

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

然后检查下fstab

```bash
cat /mnt/etc/fstab
```

应该有sda1，sda2，sda3

## 改变根路径

将根目录改变到新系统中

```bash
arch-chroot /mnt
```

## 设置时区

```bash
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

设置中国时区

```bash
hwclock --systohc --utc
```

设置时间标准为utc

## 本地化

编辑本地化文本

```bash
nano /etc/locale.gen
```

文件里面仅含注释，找到这几行，将这几个前面的`#`去掉即可

```bash
en_US.UTF-8 UTF-8
zh_CN.UTF-8 UTF-8
zh_TW.UTF-8 UTF-8
```

然后生成locale.gen文件

```bash
locale-gen
```

将系统locale设为英语，这样不容易出现乱码

```bash
echo LANG=en_US.UTF-8 > /etc/locale.conf
```

## 主机名

设置你的主机名

```bash
echo 你的主机名 > /etc/hostname
```

建议添加对应的信息到hosts

```bash
nano /etc/hosts
```

追加一行

```txt
127.0.1.1    你的主机名.localdomain    你的主机名
```

## 初始化RAM disk

用以下命令创建一个初始 RAM disk

```bash
mkinitcpio -p linux
```

## Root密码

设置root密码

```bash
passwd
```

## 安装grub引导

安装grub和efibootmgr

```bash
pacman -S grub efibootmgr
```

安装grub配置

```bash
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=grub
```

仅出现两行提示，no error report

然后生成grub配置

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

然后退回光驱的目录，卸载目录，关机，取出虚拟机光驱

```bash
exit
umount -R /mnt
poweroff
```

# 系统管理

再次开机进入系统，登录名`root`，密码是你自己设的密码

## 测试网络

```bash
ping -c 3 www.baidu.com
```

如果无法连接，则运行

```bash
dhcpcd
```

然后将dhcp设为开机启动

```bash
systemctl enable dhcpcd.service
```

将网络管理也设为开机启动

```bash
systemctl enable NetworkManager.service
```

## 安装ZSH

先安装`zsh`来替代`bash`

```bash
pacman -S zsh
```

## 新建用户

接下来就是创建用户

```bash
useradd -m -g users -G wheel -s /bin/zsh 你的用户名
```

然后设置密码

```bash
passwd 你的用户名
```

## 提升权限

安装`sudo`命令

```bash
pacman -S sudo
```

编辑sudoers，为用户添加最高权限

```bash
nano /etc/sudoers
```

找到`root ALL=(ALL) ALL`,在下面添加

```bash
你的用户名 ALL=(ALL) ALL
```

# 启动

# 用户界面

## 安装驱动

安装图形界面所需要的显卡，键盘，鼠标驱动

```bash
pacman -S xf86-video-vmware
pacman -S xf86-input-keyboard
pacman -S xf86-input-mouse
```

## 字体

先要安装字体
这里推荐几个好看的字体

```bash
pacman -S wqy-zenhei wqy-microhei
```

文泉驿中文字体

```bash
pacman -S adobe-source-code-pro-fonts ttf-dejavu
```

适合编程的等宽字体

## 安装gnome

先安装xorg桌面框架

```bash
pacman -S xorg xorg-server xorg-xinit xorg-apps
```

在安装gnome gnome-extra，其中gnome-extra可以不完全安装，只装部分组件即可

```bash
pacman -S gnome gnome-extra
```

extra推荐的包：

```bash
anjuta                  #gnome开发用的IDE
cheese                  #茄子大头贴，聊天工具必备摄像头组件
devhelp                 #gnome开发者文档浏览器
gnome-devel-docs        #gnome开发者文档
evolution               #gnome邮件软件
gedit                   #gnome文本编辑器
gnome-color-manager     #gnome色彩管理器
gnome-nettool           #gnome网络工具
file-roller             #gnome归档管理器
seahorse                #保存程序的PGP密钥
vinagre                 #gnome 桌面的远程控制服务
```

![](http://okqqc0v2n.bkt.clouddn.com/17-5-28/35268505.jpg)

更改以下设置

```bash
nano ~/.xinitrc
```

添加一行

```bash
exec gnome-session
```

接下来重启就能进入图形界面了

```bash
reboot
```

# 安装必备软件

重启后进入你自己创的用户，此后一些命令需要加`sudo`来提升权限

## 时间同步

```bash
sudo pacman -S ntp
```

## 强化zsh

安装oh-my-zsh，此神器强无敌带增强的自动补全

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

## yaourt

添加yaourt的源

```bash
sudo nano /etc/pacman.conf
```

在最后追加

```bash
[archlinuxcn]
#The Chinese Arch Linux communities packages.
SigLevel = Optional TrustAll
Server   = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
```

然后安装yaourt

```bash
sudo pacman -Sy yaourt
```

## pacman的图形化前端

安装packagekit

```bash
sudo pacman -S gnome-packagekit
```

## 输入法

安装fcitx框架

```bash
sudo pacman -S fcitx-im
sudo pacman -S fcitx-qt5
```

安装fcitx图形配置工具

```bash
pacman -S fcitx-configtool
```

在家目录`~`创建`.xprofile`并添加配置

```bash
nano .xprofile
```

```bash
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```

安装搜狗拼音

```bash
yaourt -S fcitx-sogoupinyin
```

重启后`fcitx-configtool`左下角加号添加输入法即可

## 浏览器

这里推荐安装FireFox

```bash
sudo pacman -S firefox firefox-i18n-zh-cn
```