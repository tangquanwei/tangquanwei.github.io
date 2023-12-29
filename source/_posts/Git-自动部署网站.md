---
title: Git 自动部署网站
date: 2022-08-08 15:36:48
tags:
- Git
cover:
---
# Git 自动化部署网站

## 安装配置Git服务端

```
sudo apt install git
cd
mkdir .ssh && chmod 700 .ssh
touch .ssh/authorized_keys && chmod 600 .ssh/authorized_keys
```

## 写入SSH公匙

接着，我们需要为 authorized_keys 文件添加一些开发者 SSH 公钥。
```bash
vim .ssh/authorized_keys
```
写入SSH公匙


## 创建仓库

现在我们来为开发者新建一个空仓库。可以借助带 --bare 选项的 git init 命令来做到这一点，该命令在初始化仓库时不会创建工作目录：

```bash
cd /src/git
mkdir project.git
cd project.git
git init --bare
```

## 禁用git用户shell登录

需要注意的是，目前所有（获得授权的）开发者用户都能以系统用户 git 的身份登录服务器从而获得一个普通 shell。 如果你想对此加以限制，则需要修改 /etc/passwd 文件中（git 用户所对应）的 shell 值。

借助一个名为 git-shell 的受限 shell 工具，你可以方便地将用户 git 的活动限制在与 Git 相关的范围内。 该工具随 Git 软件包一同提供。如果将 git-shell 设置为用户 git 的登录 shell（login shell）， 那么该用户便不能获得此服务器的普通 shell 访问权限。 若要使用 git-shell，需要用它替换掉 bash 或 csh，使其成为该用户的登录 shell。 为进行上述操作，首先你必须确保 git-shell 的完整路径名已存在于 /etc/shells 文件中：

```bash
cat /etc/shells # see if git-shell is already in there. If not...
which git-shell # make sure git-shell is installed on your system.
sudo -e /etc/shells # and add the path to git-shell from last command
```


现在你可以使用 chsh <username> -s <shell> 命令修改任一系统用户的 shell：
```bash

sudo chsh git -s $(which git-shell)
#如果提示没有chsh这个命令，可以去/etc/passwd将目标用户的bash shell更改为所需的内容，或者安装util-linux-user这个包
```

这样，用户 git 就只能利用 SSH 连接对 Git 仓库进行推送和拉取操作，而不能登录机器并取得普通 shell。 如果试图登录，你会发现尝试被拒绝，像这样：
```bash
ssh git@gitserver
fatal: Interactive git shell is not enabled.
hint: ~/git-shell-commands should exist and have read and execute access.
Connection to gitserver closed.
```


此时，用户仍可通过 SSH 端口转发来访问任何可达的 git 服务器。 如果你想要避免它，可编辑 authorized_keys 文件并在所有想要限制的公钥之前添加以下选项：

no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty

其结果如下：

```
cat ~/.ssh/authorized_keys
no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty ssh-rsa
AAAAB3NzaC1yc2EAAAADAQABAAABAQCB007n/ww+ouN4gSLKssMxXnBOvf9LGt4LojG6rs6h
PB09j9R/T17/x4lhJA0F3FR1rP6kYBRsWj2aThGw6HXLm9/5zytK6Ztg3RPKK+4kYjh6541N
YsnEAZuXz0jTTyAUfrtU3Z5E003C4oxOj6H0rfIF1kKI9MAQLMdpGW1GYEIgS9EzSdfd8AcC
```

现在，网络相关的 Git 命令依然能够正常工作，但是开发者用户已经无法得到一个普通 shell 了。 正如输出信息所提示的，你也可以在 git 用户的主目录下建立一个目录，来对 git-shell 命令进行一定程度的自定义。 比如，你可以限制掉某些本应被服务器接受的 Git 命令，或者对刚才的 SSH 拒绝登录信息进行自定义，这样，当有开发者用户以类似方式尝试登录时，便会看到你的信息。 要了解更多有关自定义 shell 的信息，请运行 git help shell。

## 利用git push向服务器一键部署代码

进入我们创建的项目仓库
```Bash
cd /src/git/project.git
cd hooks
vim post-receive
```


写入如下代码：
```Bash
#!/bin/sh
git --work-tree=/srv/web/xxx.com checkout -f master #这个目录是代码存放目录不是仓库目录
echo "The project has been successfully deployed to this server!"
```
```Bash
chmod +x post-receive
```


这个挂钩本质是一个脚本，当这个文件在repo/hooks/文件夹内，名字为 post-receive 并且linux的x属性（可执行）为真的时候，git会在收到push后执行它

OK！现在上传的代码会在 /src/web/xxx.com 这个目录里面看见，我们只需要把网站根目录设置在这里就可以了