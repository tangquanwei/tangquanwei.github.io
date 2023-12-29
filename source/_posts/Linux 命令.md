---
title: Linux 常用命令
date: 2023-03-13 10:52:31
tags: Linux
---

# LINUX 命令

## LINUX 终端快捷键

```
Ctrl + Shift + C 复制
Ctrl + Shift + V 粘贴 

Ctrl + Shift + + 放大字体
Ctrl + Shift + - 缩小字体

Ctrl + l 清屏

Ctrl + a  移动光标到命令行首
Ctrl + e  移动光标到命令行尾 

Ctrl + b  同键盘左键，向左移动光标
Ctrl + f  同键盘右键，向右移动光标

Alt + b  向前移动一个词的距离
Alt + f  向后移动一个词的距离 

Ctrl + w  剪切光标之前的一个词
Alt + d  剪切光标之后的一个词

Ctrl + u  从当前光标所在位置向左剪切全部命令
Ctrl + k  从当前光标所在位置向右剪切全部命令

Ctrl + t  交换光标前的最后两个字符
Alt + t  交换当前单词和前一个单词的位置

Ctrl + r  查看历史命令，需要输入命令的起始字母，剩下的部分自动补全

Ctrl + p  显示上一条命令，同向上箭头
Ctrl + n  显示下一条命令，同向下箭头

Tab  按一次补全，按两次列出所有相关信息

Esc + .  插入最后一个参数，也就是上一个命令的最后一个参数或者叫字符串 
```

<!-- more -->

## 系统管理

```bash
# 关闭
shutdown 
  -H Halt
  -P -h  poweroff
  -c cancle
  -r reboot
  -k 警告

# 将数据由内存同步到硬盘中
sync 
```

## 文件管理

```bash
# 确定文件类型
file fname

# 切换目录
# ./ 当前目录
# ../ 父目录
cd 相对路径or绝对路径

# 回到上次的目录
cd -

# print work dir
pwd

# 列出文件目录
ls 
  -l 长格式
  -R 递归
  -a 包含隐藏文件
  -h 人性化的
  -F 添加标识符

# 创建目录
mkdir 
  -p make parent directories as needed

# 创建文件
touch

# 文件状态
stat
  Access: 文件上一次打开的时间
  Modify: 文件内容上一次变动的时间
  Change: ctime 指 inode 上一次变动的时间
   Birth：文件创建时间

# 复制
cp
  -r 递归复制目录
  -p 保持权限和属性
  -a rp

# 移动
mv

# 删除
rm
  -r 递归
  -i 提示
  -f 强制

# 查看
cat/tac
  -n number all output lines  
  -A show-all   
  -e show-nonprinting show-ends   

# 带行号
nl
  -<line>

# 一页,可以滚动
more/less

ldd print shared object dependencies
```

## 历史命令

```bash
history  查看历史命令，按顺序全部显示出来，有对应的编号

!num  执行history历史命令列表中第num条命令

!!  执行上一条命令
!-1 执行上一条命令

!?string?  执行含有string字符串的最新命令

ls !$  执行命令ls，并以上一条命令的最后一个字符串为其参数
```

## 内容查找

```bash
grep [OPTION...] PATTERNS [FILE...]
```

## 路径查找
```bash
which

where

whereis

locate

find [path] [expression]

    -name
    -user -uid -group -gid
    -perm
    -atime -mtime -ctime
    -i(gnore-case)
    -a(nd)

    -size n[cwbkMG]
          `b'    for 512-byte blocks (this is the default if no suffix is used)
          `c'    for bytes
          `w'    for two-byte words
          `k'    for kibibytes (KiB, units of 1024 bytes)
          `M'    for mebibytes (MiB, units of 1024 * 1024 = 1048576 bytes)
          `G'    for gibibytes (GiB, units of 1024 * 1024 * 1024 = 1073741824 bytes)
    +n     for greater than n,
    -n     for less than n,
    n      for exactly n.
```
## 打包压缩

```bash
# zip往压缩包里添加文件
zip <name>.zip <file1> <file2>
upzip <name>.zip -d <dir>

# 只能压缩单个文件
gzip <name>.gz <file1> 
gunzip <name>.gz 

bzip2 <name>
bunzip2 <name>.bz2

xz <name>
unxz <name>.xz

# 打包不压缩
tar -cvf <name>.tar <file>

# 打包gzip压缩
tar -cvzf <name>.tar.gz <file>

# 打包bzip2压缩 
tar -cvjf <name>.tar.bz2 <file>

# 打包xz压缩
tar -cvJf <name>.tar.xz <file>

# 解压到指定目录
tar -xvf <name> -C <dir>
```

## 链接

ln src dest

硬链接 链接到inode

软链接 --symbolic 

![](2023-06-24-11-21-34.png)