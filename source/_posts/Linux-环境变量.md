---
title: Linux 环境变量
date: 2022-09-24 15:52:31
tags: Linux
---
# LINUX 环境变量

## 查看

用户级别环境变量定义文件：

  ~/.bashrc
  ~/.profile
  ~/.bash_profile

系统级别环境变量定义文件：

  /etc/bashrc
  /etc/profile
  /etc/bash_profile
  /etc/environment

```bash
export     # 命令显示当前系统定义的所有环境变量

echo $PATH # 命令输出当前的PATH环境变量的值
```

其中PATH变量定义了运行命令的查找路径，以冒号:分割
使用export定义的时候可加双引号也可不加

## 添加

### 1. export PATH

使用export命令直接修改PATH的值

```bash
export PATH=/home/uusama/mysql/bin:$PATH

export PATH=$PATH:/home/uusama/mysql/bin
```

注意事项：

生效时间：立即生效
生效期限：当前终端有效，窗口关闭后无效
生效范围：仅对当前用户有效
配置的环境变量中不要忘了加上原来的配置，即$PATH部分，避免覆盖原来配置

### 2. vim ~/.bashrc

通过修改用户目录下的~/.bashrc文件进行配置：

```bash
vim ~/.bashrc

# 在最后一行加上
export PATH=$PATH:/home/uusama/mysql/bin
```

注意事项：

生效时间：使用相同的用户打开新的终端时生效，或者手动source ~/.bashrc生效
生效期限：永久有效
生效范围：仅对当前用户有效
如果有后续的环境变量加载文件覆盖了PATH定义，则可能不生效

### 3. vim ~/.bash_profile

和修改~/.bashrc文件类似，也是要在文件最后加上新的路径即可：

```bash
vim ~/.bash_profile

# 在最后一行加上
export PATH=$PATH:/home/uusama/mysql/bin
```

注意事项：

生效时间：使用相同的用户打开新的终端时生效，或者手动source ~/.bash_profile生效
生效期限：永久有效
生效范围：仅对当前用户有效
如果没有~/.bash_profile文件，则可以编辑~/.profile文件或者新建一个

### 4. vim /etc/bashrc

该方法是修改系统配置，需要管理员权限（如root）或者对该文件的写入权限：

```bash
sudo vim /etc/bashrc

# 在最后一行加上
export PATH=$PATH:/home/uusama/mysql/bin
```

注意事项：

生效时间：新开终端生效，或者手动source /etc/bashrc生效
生效期限：永久有效
生效范围：对所有用户有效

### 5. vim /etc/profile

该方法修改系统配置，需要管理员权限或者对该文件的写入权限，和vim /etc/bashrc类似：

```bash
sudo vim /etc/profile

# 在最后一行加上
export PATH=$PATH:/home/uusama/mysql/bin
```

注意事项：

生效时间：新开终端生效，或者手动source /etc/profile生效
生效期限：永久有效
生效范围：对所有用户有效

### 6. vim /etc/environment

该方法是修改系统环境配置文件，需要管理员权限或者对该文件的写入权限：

```bash
sudo vim /etc/environment

# 在最后一行加上
export PATH=$PATH:/home/uusama/mysql/bin
```

注意事项：

生效时间：新开终端生效，或者手动source /etc/environment生效
生效期限：永久有效
生效范围：对所有用户有效

