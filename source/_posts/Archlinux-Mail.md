---
title: Archlinux Mail
date: 2023-04-04 21:49:23
tags: 
- Linux
cover: mail.jpg
---

# Archlinux Mail

## 安装

```bash
sudo pacman -S s-nail
```

## 编辑配置文件

sudo vim /etc/mail.rc
```
set from="<USERNAME>@qq.com"
set smtp-auth=login
set mta=smtps://<USERNAME>:<QQ邮箱授权码>@smtp.qq.com:465	#smtp服务器端口是465
set v15-compat	#必须要
set nss-config-dir=/root/.certs
```

## 获得邮箱的SSL证书并存放到本地

最后一行的nss-config-dir就是制定的存放QQ邮箱SSL证书的位置

手动的获取QQ邮箱的证书保存到本地指定的目录里以备调用和验证

```bash
su root
mkdir -p /root/.certs/
echo -n | openssl s_client -connect smtp.qq.com:465 | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > ~/.certs/qq.crt
certutil -A -n "GeoTrust SSL CA" -t "C,," -d ~/.certs -i ~/.certs/qq.crt
certutil -A -n "GeoTrust Global CA" -t "C,," -d ~/.certs -i ~/.certs/qq.crt
certutil -L -d /root/.certs
cd /root/.certs
certutil -A -n "GeoTrust SSL CA - G3" -t "Pu,Pu,Pu" -d ./ -i qq.crt
```
![](2023-04-04_21-56.png)

## 测试

```bash
mailx -s "TEST FROM LINUX" xxx@qq.com < ./mail_file.txt
```