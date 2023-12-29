---
title: RHEL YUM 本地源配置
date: 2023-05-18 10:04:09
tags: LINUX
---

# RHEL YUM 本地源配置

## 挂载本地源

```bash
# 查看挂载的iso
mount | grep iso

# 查看挂载文件（块）
l /dev/sr0

# 创建文件（这里是普通用户）
sudo mkdir /mnt/cdrom

# 挂载
sudo mount /dev/sr0 /mnt/cdrom/

# 查看被挂载的目录
ls /mnt/cdrom/
```

![](2023-05-18-10-04-26.png)

## 备份

下面的命令以root身份执行
```bash
mkdir /root/yum.repo

mv /etc/yum.repos.d/* /root/yum.repo

```

## 配置本地源

```bash
vim /etc/yum.repos.d/local.repo
```
写入了：
```
[local]
name=quanwei_local
baseurl=file:///mnt/cdrom/AppStream
enabled=1
gpgcheck=0
```
![](2023-05-18-10-10-50.png)
如果没写AppStream的话会说找不到matadata

完整配置
![](2023-05-18-10-45-28.png)
## 清除缓存

![](2023-05-18-10-15-20.png)

## 测试安装
![](2023-05-18-10-28-51.png)