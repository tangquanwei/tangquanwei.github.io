---
title: Linux 计划任务
date: 2022-09-24 16:44:33
tags: Linux
---

# 计划任务

定时任务: cron

一次性计划任务: at

<!-- more -->
## Cron 

### 安装、启动

Ubuntu

```bash
# 安装
apt-get install cron

# 启动
service cron start

# 重启
service cron restart

# 停止
service cron stop

# 检查状态
service cron status

# 查询cron可用的命令
service cron

# 检查Cronta工具是否安装
crontab -l
```

Archlinux

```bash
# 安装
sudo pacman -S cronie

# 启动
systemctl start cronie.service

# 重启
systemctl restart cronie.service

# 停止
systemctl stop cronie.service

# 检查状态
systemctl status cronie.service
```

### 2. 配置、使用

都是一样的

Usage:

    crontab [options] file
    crontab [options]
    crontab -n [hostname]

Options:

    -u <user>  define user
    -e         edit user's crontab
    -l         list user's crontab
    -r         delete user's crontab
    -i         prompt before deleting
    -n <host>  set host in cluster to run users' crontabs
    -c         get host in cluster to run users' crontabs
    -T <file>  test a crontab file syntax
    -V         print version and exit
    -x <mask>  enable debugging


| field        | allowed values                       |
| ------------ | ------------------------------------ |
| minute       | 0-59                                 |
| hour         | 0-23                                 |
| day of month | 1-31                                 |
| month        | 1-12 (or names, see below)           |
| day of week  | 0-7 (0 or 7 is Sunday, or use names) |

    *    *    *    *    *
    -    -    -    -    -
    |    |    |    |    |
    |    |    |    |    +----- 星期中星期几 (0 - 6) (星期天 为0)
    |    |    |    +---------- 月份 (1 - 12) 
    |    |    +--------------- 一个月中的第几天 (1 - 31)
    |    +-------------------- 小时 (0 - 23)
    +------------------------- 分钟 (0 - 59)

    *：表示取值范围中的每一个数字
    -：做连续区间表达式的，要想表示1~7，则可以写成：1-7
    /：表示每多少个，例如：想每10分钟一次，则可以在分的位置写：*/10
    ,：表示多个取值，比如想在1点，2点6点执行，则可以在时的位置写：1,2,6

| nicknames | time and date fields               |
| --------- | ---------------------------------- |
| @reboot   | Run once after reboot.             |
| @yearly   | Run once a year, ie.  "0 0 1 1 *". |
| @annually | Run once a year, ie.  "0 0 1 1 *". |
| @monthly  | Run once a month, ie. "0 0 1 * *". |
| @weekly   | Run once a week, ie.  "0 0 * * 0". |
| @daily    | Run once a day, ie.   "0 0 * * *". |
| @hourly   | Run once an hour, ie. "0 * * * *". |

编辑
```bash
crontab -e
```

文件

* /etc/crontab      main system crontab file.   
* /var/spool/cron/  a directory for storing crontabs defined by users. 
* /etc/cron.d/      a directory for storing system crontabs.
* /etc/cron.allow 允许使用的用户 
* /etc/cron.deny  拒绝使用的用户
  

## at 在将来某个时刻执行

安装

    sudo pacman -S at  
    systemctl start atd.service

创建at作业

    at <time><increment>
    <command>
    ctrl+d

time

    midnight  Indicates the time 12:00 am (00:00).
    noon      Indicates the time 12:00 pm.
    now       Indicates  the current day and time. 
    today     Indicates the current day.
    tomorrow  Indicates the day following the current day.
increment    

    + N minutes, hours, days, weeks, months, years

eg.

    at now +1 minute
    date >> a.txt
    ctrl+d

![](2023-05-22-17-07-46.png)

一分钟后

![](2023-05-22-17-08-24.png)

显示作业

    at -l
![](2023-05-22-17-11-14.png)

删除

    at -d <id>
![](2023-05-22-17-11-34.png)