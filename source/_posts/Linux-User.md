---
title: Linux User Group
date: 2023-03-24 21:33:00
tags: Linux
---
# Linux User

Linux 下用户和组管理

## 4个文件

1. /etc/passwd : 保存用户账户信息
2. /etc/shadow : 用户账户密码 
3. /etc/group ： 账户分组信息
4. /etc/gshadow ：组口令、组管理员

## 获取用户和组相关信息

```bash
# 当前用户的信息
id

# 当前登陆的用户名
users

# 当前用户的组
groups

who/whoami

cat /etc/passwd
```

## 用户管理

###  添加用户

1. useradd
```bash
useradd <arg> username
  -c comment
  -d home-dir
  -e expire-date
  -g user-group-name
  -G supplementary-group
  -s shell-path
  -u uid
  -D username
```

2. adduser
```
adduser
```

### 修改用户

1. usermod
```
usermod <arg> username
  -l newname 
  -L lock  
  -U unlock
  -u uid
  -G groups
```

2. passwd

```
passwd <arg> username
  -S(tatus) 
  -l(ock)
  -u(nlock)
  -d(elete-passwd) 
  -e(xpire)
```

无参数即设置密码

3. userdel

```
userdel [-rf] username
```

-r 删除/etc/passwd, /etc/shadow, /etc/group, /etc/gshadow, 的记录，同时删除用户的主目录
 /var/spool/mail

## 组管理

1. groupadd 添加组


```
groupadd <arg> groupname
  -g gid
  -p password
  -U username,...
  -r (Create a system group.)
```

2. groupmod 修改组

```
groupmod <arg> groupname
  -g gid
  -n gname
  -p passwd
```

3. groupdel 删除组

```
groupdel [-f] groupname
```

4. gpasswd 增删用户到组

```
gpasswd <arg> username groupname
  -a(dd)
  -d(elete)
  -A(dmin)
  -M(embers) username,...
```