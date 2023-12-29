---
title: Linux 复习
date: 2023-06-12 18:13:33
tags:
cover:
---

1. 选择题(15分)

新建文件 : touch mkdir
cat echo vim
rm mv


2. 简答题(16分)

[yum 本地源配置]()

手动添加用户,系统变化的6个地方:

    /etc/passwd
    /etc/shadow
    /etc/gpasswd
    /etc/gshadow
    /home/..
    /var/spool/mail

3. 操作题(48分)

权限 字母数字

chmod

cron

cut sed awk

su -  sudo su 描述

改变用户名,组名
usermod 

硬链接,软链接,如何建立,作用

打包,压缩

增删改查

    touch\mkdir
    rm -rf
    vi
    find\grep

别名 alias

4. Shell 编程(16分)

```bash
#!/bin/bash
# 
# 输入用户名,数量,密码 
# 创建对应数量的用户并设置初始密码
# 

# 提醒读入
read -p "input username" name
read -p "input number of user" num
read -p "input passwd of them" pswd

# 三个输入都不为空
if [ ! -z "$name" -a ! -z "$num" -a ! -z "$pswd" ];then
  # 除去数字字符
  y=$(echo $num | sed 's/[0-9]//g')
  echo "y:[$y]"
  # 判断结果是否为空,即num是否全由数字构成
  if [ -z "$y" ];then
    # 创建 num 个用户
    for((i=1;i<=${num};i++));do 
      /usr/bin/useradd $name$i > /dev/null
      echo $pswd |/usr/bin/passwd --stdin $name$i & 2> /dev/null
      # echo ${name}${i}:${pswd} | chpasswd # 我的系统不能执行上一句,用这句替代
    done
  fi 
fi 
```

执行结果

![](2023-06-12-19-25-43.png)

![](2023-06-12-19-25-52.png)


```bash
#!/bin/bash
#
# 输入用户名,数量,密码 
# 创建对应数量的用户并设置初始密码
# 下次登陆时修改密码 
#

read -p "input username: " name
read -p "input number of user: " num
read -p "input passwd of them: " pswd

n=1
# n<=num
while [ $n -le $num ];do 
  # ${name}${n} 用户名和后面的序号的连接
  # -e 设置密码过期时间
  /usr/bin/useradd ${name}${n} -e 0

  # 设置密码
  # echo $pswd |/usr/bin/passwd --stdin $name$n  
  echo ${name}${n}:${pswd} | chpasswd # 我的系统不能执行上一句,用这句替

  # 变量自增
  n=$(($n+1))
  # n=`expr $n + 1`
done
```


```bash
# 或者
for((n=1;n<=${num};n++));do
  /usr/bin/useradd ${name}${n}
  echo $pswd |/usr/bin/passwd --stdin $name$n
done
```