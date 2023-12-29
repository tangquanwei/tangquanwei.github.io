---
title: Linux 使用 Windows 字体
date: 2023-01-08 15:52:28
tags: Linux
cover:
---
# Linux使用Windows字体

## 复制Windows系统字体

Windows系统里的字体目录为：C:\Windows\Fonts

注意：该文件夹里有三种后缀的文件：.fon，.ttf，.ttc，我们只需要复制.ttf和.ttc后缀的文件

```bash
# 在/usr/share/fonts/下新建目录：win_fonts
sudo mkdir /usr/share/fonts/win_fonts

# 将Windows系统Fonts目录里的所有文件全部复制到Linux
sudo cp /Path/to/Windows/Fonts/*.ttf /usr/share/fonts/win_fonts
sudo cp /Path/to/Windows/Fonts/*.ttc /usr/share/fonts/win_fonts
```

## 生成字体的索引信息

```bash
sudo mkfontscale
sudo mkfontdir
```

## 更新字体缓存
```bash
sudo fc-cache
```
