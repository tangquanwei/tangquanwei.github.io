---
title: ArchLinux 触摸板调教
date: 2023-06-08 15:30:46
tags: Linux
cover:
---

## 开始之前

    OS：Archlinux
    WM：bspwm

## 安装触摸板驱动

sudo pacman -S xf86-input-libinput

配置文件

```Bash
sudo vim /etc/X11/xorg.conf.d/30-touchpad.conf
```

```Bash
Section "InputClass"
    Identifier "touchpad"
    Driver "libinput"
    MatchIsTouchpad "on"
    Option "Tapping" "on"
    Option "TappingButtonMap" "lmr"
EndSection
```

写入配置之后记得重新启动一下


## 配置触摸板手势

安装配置 libinput-gestures

```Bash
sudo pacman -S libinput-gestures
```

libinput-gestures的文档中说了：必须是input组的成员才能具有读取触摸板设备的权限，所以需要添加用户到input

```bash
sudo gpasswd -a $USER input
```

退出登录后生效（或者重启）

## 写入配置

```bash
vim .config/libinput-gestures.conf
```

```
gesture swipe left 4 bspc node -s west
gesture swipe right 4 bspc node -s east
gesture swipe up 4 bspc desktop -f prev.local
gesture swipe down 4 bspc desktop -f next.local
gesture swipe left 3 bspc node -f west
gesture swipe right 3 bspc node -f east
gesture swipe down 3 flameshot gui
gesture swipe up 3 rofi -show drun
```

```
libinput-gestures-setup autostart
libinput-gestures-setup start
```