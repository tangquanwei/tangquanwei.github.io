---
title: Linux 系统管理
date: 2023-05-22 16:47:12
tags: Linux
---

# Linux 系统管理

## PS 查看进程状态

```bash
ps [options]
ps aux |grep <name>
ps ef

ps -u <username> 
```

Options:

    -A,-e,a     Select all processes.  Identical to -e.
    -a     Select all processes except both session leaders and processes not associated with a terminal.
    -d     Select all processes except session leaders.
    g      Really all, even session leaders.  
    -N,--deselect     Select all processes except those that fulfill the specified conditions (negates the selection).  
    T,t      Select all processes associated with this terminal.  
    r      Restrict the selection to only running processes.
    x      list all processes owned by you and others.

结果：


    USER: 进程拥有者
    PID: pid
    %CPU: 占用的 CPU 使用率
    %MEM: 占用的内存使用率
    VSZ: 占用的虚拟内存大小
    RSS: 占用的内存大小
    TTY: 终端的次要装置号码 (minor device number of tty)

    STAT: 该进程的状态:
        D: 无法中断的休眠状态 (通常 IO 的进程)
        R: 正在执行中
        S: 静止状态
        T: 暂停执行
        Z: 不存在但暂时无法消除
        W: 没有足够的内存分页可分配
        <: 高优先序的进程
        N: 低优先序的进程
        L: 有内存分页分配并锁在内存内 (实时系统或捱A I/O)
    START: 进程开始时间
    TIME: 执行的时间
    COMMAND:所执行的指令

## free 查看内存状态

```bash
free
```
## sleep 命令延迟执行

```bash
sleep time; cmd
```

## kill 杀死进程

```bash
kill [-<signal>] pid


pgrep [options] pattern
pgrep -u root sshd # list the processes called sshd AND owned by root

pkill [options] pattern # send the specified signal to each process 

pwait [options] pattern # wait for each process instead of listing them on stdout
```
## renice (alter priority of running processes)

    -n, --priority priority
    -g, --pgrp
    -p, --pid
    -u, --user

change the priority of the processes with PIDs 987 and 32, plus all processes owned by the users daemon and root:

       renice +1 987 -u daemon root -p 32

## top 显示进程实时状态

top  
h 查看帮助  
man top 查看man page  

一些指标

    %CPU  --  CPU Usage
    %MEM  --  Memory Usage (RES)
    CODE  --  Code Size (KiB)
    COMMAND  --  Command Name or Command Line
    DATA  --  Data + Stack Size (KiB)
    Flags  --  Task Flags
    NI  --  Nice Value
    NU  --  Last known NUMA node
    P  --  Last used CPU (SMP)
    PGRP  --  Process Group Id
    PR  --  Priority
    RES  --  Resident Memory Size (KiB)
    SHR  --  Shared Memory Size (KiB)
    SID  --  Session Id
    SUID  --  Saved User Id
    SWAP  --  Swapped Size (KiB)
    TGID  --  Thread Group Id
    TIME  --  CPU Time
    TIME+  --  CPU Time, hundredths
    USED  --  Memory in Use (KiB)

    S  --  Process Status
        The status of the task which can be one of:
            D = uninterruptible sleep
            I = idle
            R = running
            S = sleeping
            T = stopped by job control signal
            t = stopped by debugger during trace
            Z = zombie

    us, user    : time running un-niced user processes
    sy, system  : time running kernel processes
    ni, nice    : time running niced user processes
    id, idle    : time spent in the kernel idle handler
    wa, IO-wait : time waiting for I/O completion
    hi : time spent servicing hardware interrupts
    si : time spent servicing software interrupts
    st : time stolen from this vm by the hypervisor

htop

btop++

## 前后台切换

```
            ctrl+z ->             bg ->
Foreground ----------- Stopped ---------- Background &
                  <----- fg  -------                     
```
 
jobs 查看挂起到后台的进程

## systemctl 服务管理

```bash
# 修改服务状态
systemctl {status
            |start
            |stop
            |restart
            |reload
            |enable
            |disable
            |kill
            } <name[.service]>

# 修改系统状态
systemctl { reboot # 重启系统
|poweroff # 关闭系统，切断电源
|halt # CPU停止工作
|suspend # 暂停系统(待机)
|hibernate # 让系统进入冬眠状态
|hybrid-sleep # 让系统进入交互式休眠状态
|rescue # 启动进入救援状态（单用户状态）
}    
```

```bash
systemctl enable clamd@scan.service
# 等同于
ln -s '/usr/lib/systemd/system/clamd@scan.service' '/etc/systemd/system/multi-user.target.wants/clamd@scan.service'
```

Unit(系统资源):

    Service unit：系统服务
    Target unit：多个 Unit 构成的一个组
    Device Unit：硬件设备
    Mount Unit：文件系统的挂载点
    Automount Unit：自动挂载点
    Path Unit：文件或路径
    Scope Unit：不是由 Systemd 启动的外部进程
    Slice Unit：进程组
    Snapshot Unit：Systemd 快照，可以切回某个快照
    Socket Unit：进程间通信的 socket
    Swap Unit：swap 文件
    Timer Unit：定时器

```bash
# 列出正在运行的 Unit
systemctl list-units

# 列出所有Unit，包括没有找到配置文件的或者启动失败的
systemctl list-units --all

# 列出所有没有运行的 Unit
systemctl list-units --all --state=inactive

# 列出所有加载失败的 Unit
systemctl list-units --failed

# 列出所有正在运行的、类型为 service 的 Unit
systemctl list-units --type=service


# 显示系统状态
systemctl status

# 显示单个 Unit 的状态
sysystemctl status bluetooth.service

# 显示远程主机的某个 Unit 的状态
systemctl -H root@rhel7.example.com status httpd.service

# 显示某个 Unit 是否正在运行
systemctl is-active application.service

# 显示某个 Unit 是否处于启动失败状态
systemctl is-failed application.service

# 显示某个 Unit 服务是否建立了启动链接
systemctl is-enabled application.service
```


配置文件的格式

    systemctl cat atd.service

        [Unit]
        Description=ATD daemon

        [Service]
        Type=forking
        ExecStart=/usr/bin/atd

        [Install]
        WantedBy=multi-user.target

Target : 一个 Unit 组，包含许多相关的 Unit
```bash
# 查看当前系统的所有 Target
systemctl list-unit-files --type=target

# 查看一个 Target 包含的所有 Unit
systemctl list-dependencies multi-user.target

# 查看启动时的默认 Target
systemctl get-default

# 设置启动时的默认 Target
sudo systemctl set-default multi-user.target

# 切换 Target 时，默认不关闭前一个 Target 启动的进程，
# systemctl isolate 命令改变这种行为，
# 关闭前一个 Target 里面所有不属于后一个 Target 的进程
sudo systemctl isolate multi-user.target
```

日志
```bash
# 查看所有日志（默认情况下 ，只保存本次启动的日志）
sudo journalctl

# 查看内核日志（不显示应用日志）
sudo journalctl -k

# 查看系统本次启动的日志
sudo journalctl -b
sudo journalctl -b -0

# 查看上一次启动的日志（需更改设置）
sudo journalctl -b -1

# 查看指定时间的日志
sudo journalctl --since="2012-10-30 18:17:16"
sudo journalctl --since "20 min ago"
sudo journalctl --since yesterday
sudo journalctl --since "2015-01-10" --until "2015-01-11 03:00"
sudo journalctl --since 09:00 --until "1 hour ago"

# 显示尾部的最新10行日志
sudo journalctl -n

# 显示尾部指定行数的日志
sudo journalctl -n 20

# 实时滚动显示最新日志
sudo journalctl -f

# 查看指定服务的日志
sudo journalctl /usr/lib/systemd/systemd

# 查看指定进程的日志
sudo journalctl _PID=1

# 查看某个路径的脚本的日志
sudo journalctl /usr/bin/bash

# 查看指定用户的日志
sudo journalctl _UID=33 --since today

# 查看某个 Unit 的日志
sudo journalctl -u nginx.service
sudo journalctl -u nginx.service --since today

# 实时滚动显示某个 Unit 的最新日志
sudo journalctl -u nginx.service -f

# 合并显示多个 Unit 的日志
journalctl -u nginx.service -u php-fpm.service --since today

# 查看指定优先级（及其以上级别）的日志，共有8级
# 0: emerg
# 1: alert
# 2: crit
# 3: err
# 4: warning
# 5: notice
# 6: info
# 7: debug
sudo journalctl -p err -b

# 日志默认分页输出，--no-pager 改为正常的标准输出
sudo journalctl --no-pager

# 以 JSON 格式（单行）输出
sudo journalctl -b -u nginx.service -o json

# 以 JSON 格式（多行）输出，可读性更好
sudo journalctl -b -u nginx.serviceqq
 -o json-pretty

# 显示日志占据的硬盘空间
sudo journalctl --disk-usage

# 指定日志文件占据的最大空间
sudo journalctl --vacuum-size=1G

# 指定日志文件保存多久
sudo journalctl --vacuum-time=1years

```

## 其他系统管理命令

systemd-analyze命令用于查看启动耗时

```bash
systemd-analyze # 查看启动耗时                                                                                      
systemd-analyze { blame # 查看每个服务的启动耗时
|critical-chain # 显示瀑布状的启动过程流
|critical-chain atd.service # 显示指定服务的启动流
}
```

hostnamectl命令用于查看当前主机的信息
```bash
# 显示当前主机的信息
hostnamectl
# 设置主机名
sudo hostnamectl set-hostname rhel7
```

localectl命令用于查看本地化设置
```bash
# 查看本地化设置
localectl

# 设置本地化参数
localectl set-locale LANG=en_GB.utf8
localectl set-keymap en_GB

```

timedatectl命令用于查看当前时区设置
```bash
# 查看当前时区设置
timedatectl

# 显示所有可用的时区
timedatectl list-timezones                                                                                   

# 设置当前时区
timedatectl set-timezone America/New_York
timedatectl set-time YYYY-MM-DD
timedatectl set-time HH:MM:SS

```

loginctl命令用于查看当前登录的用户
```bash
# 列出当前session
loginctl list-sessions
# 列出当前登录用户
loginctl list-users
# 列出显示指定用户的信息
loginctl show-user ruanyf
```