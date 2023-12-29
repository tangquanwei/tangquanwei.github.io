---
title: Archlinux BspWM 外接显示器
date: 2023-06-08 15:43:50
tags: Linux
cover:
---

## 了解

```bash
man xrandr # 查看帮助

xrandr # 获取显示器接口信息
```

## 显示配置

非必要

```bash
vim /etc/X11/xorg.conf.d/10-monitor.conf
```

```conf
Section "Monitor"
        Identifier "eDP-1-1"
        Option "Primary" "true"
        Option "DPMS" "true"
        Option "PreferredMode" "1920x1080_60.00"
EndSection

Section "Monitor"
        Identifier "HDMI-0"
        Option "DPMS" "true"
        Option "PreferredMode" "1920x1080_60.00"
        Option "RightOf" "eDP-1-1"
EndSection
```


## 切换

```bash
#! /bin/bash

if [ $# -lt 1 ]; then
  echo 'Usage screen <action>'
  echo 'action:'
  echo '  (s)ingle   仅[电脑]显示器'
  echo '  (e)xtend   双显示器[扩展]'
  echo '  s(y)nc     双显示器[同步]'
fi

intern=eDP-1-1
extern=HDMI-0

case $1 in 
  single|s)
    echo '仅[电脑]显示器'
    xrandr --output "$intern" --auto --output "$extern" --off
    # bspc monitor -d I II III IV V VI VII VIII IX X
    ;;
  extend|e)
    echo '双显示器[扩展]'
    xrandr --output "$intern" --auto --output "$extern" --primary --auto --right-of "$intern"
    bspc monitor "$intern" -d I II III IV V 
    bspc monitor HDMI-0 -d I II III IV V 
    ;;
  left|l)
    echo '双显示器[扩展 LEFT]'
    xrandr --output "$intern" --primary --auto --output "$extern"  --auto --right-of "$intern"
    bspc monitor "$intern" -d I II III IV V 
    bspc monitor HDMI-0 -d I II III IV V 
    ;;
  sync|y|d)
    echo '双显示器[同步]'
    xrandr --output "$intern" --auto --output "$extern" --same-as "$intern" --auto  
    bspc monitor "$intern" -d I II III IV V 
    bspc monitor HDMI-0 -d I II III IV V    
     ;;
  *)
    echo '未知参数[' $1 ']'
    ;;
esac
```