---
title: Linux 权限
date: 2023-05-08 16:29:16
tags: Linux
---

# Linux 

## 普通权限

### 三种角色：
**u(user,owner)**
**g(roup)**
**o(ther)**

### 三种权限:
**r(ead)**
**w(rite)**
**(e)x(ecute)**


| privilege   | file     | directary        |
| ----------- | -------- | ---------------- |
| r(ead)      | 读取内容 | 列出文件         |
|             | cat      | ls               |
| w(rite)     | 修改内容 | 增删文件         |
|             | vi       | rm、touch、mkdir |
| (e)x(ecute) | 执行     | 可进入           |
|             | sh       | cd               |

### 三种动作：
```
+ (add)
- (remove)
= (set)
```

eg.

u+rw 
角色：所有者，动作：添加，权限：读和写

## 八(二)进制方式：

|     | binary | octal |
| --- | ------ | ----- |
| r-- | 100    | 4     |
| -w- | 010    | 2     |
| --x | 001    | 1     |

eg.  

rwxr-xr-x 755  
rwxrwxrwx 777

## 命令

1. 更改文件权限

```bash
chmod [ugoa][+-=][rwx] file
# eg.
chmod u+rx a.txt

chomd [octal] file
# eg.
chmod 755 a.txt
```
-R(ecursive) 递归修改权限

2. 更改文件所有者
```bash
chown [owner[:[group]]] filename

# eg.
chown root a.txt
chown root: a.txt
chown root:wheel a.txt

```

1. 更改文件所属组
```bash
chgrp group filename

# eg.
chgrp wheel a.txt
```

## 附加权限

| privilege | octal | file               | directary                                |
| --------- | ----- | ------------------ | ---------------------------------------- |
| (s)uid    | 4000  | 继承所有者权限     |                                          |
| (s)gid    | 2000  | 继承所有者组权限   | 该目录下创建的所有文件(目录)属于目录属组 |
| s(t)icky  | 1000  | 程序完成后保存副本 | 只有目录所有者或root才能删除目录         |

当可执行文件设置suid/sgid权限时，任何用户执行改文件时都会获得文件所有者、组账号的对应身份  

s suid (4)  
g sgid (2)   

t sticky (1) 唯root或owner可移动、删除  

eg.

chmod +t file  
chmod 1755 file

![](2023-05-21-13-15-56.png)

![](2023-05-21-13-19-41.png)
## umask

```bash
umask
```

创建文件时的默认权限

  文件权限 = 0666 - umask

  目录权限 = 0777 - umask

可见文件比目录多了0111，即执行的权限
why？安全也


## 文件属性

lsattr 列出属性

![](2023-05-21-13-40-02.png)


chattr

```bash
chattr [-RV][-v<版本编号>][{+|-|=}<属性>][文件or目录...]
```

    a：让文件或目录仅供附加用途。
    b：不更新文件或目录的最后存取时间。
    c：将文件或目录压缩后存放。
    d：将文件或目录排除在dump操作之外。
    i：不得任意更动文件或目录。
    s：保密性删除文件或目录。
    S：即时更新文件或目录。
    u：预防意外删除。

![](2023-05-21-13-45-20.png)