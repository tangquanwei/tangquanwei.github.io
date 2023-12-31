---
title: Git FQA
tags: Git 
date: 2023-12-28 21:35:43
cover:
---

## .gitignore不生效

如果某些文件已经被纳入了版本管理中，就算是在.gitignore中已经声明了忽略路径也是不起作用的，
这时候我们就应该先把本地缓存删除，然后再进行git的提交，这样就不会出现忽略的文件了

解决方法: git清除本地缓存（改变成未track状态），然后再提交:

```bash
git rm -r --cached .
git add .
git commit -m 'update .gitignore'
git push -u origin master
```

## Git 的使用——提交避免输入用户名和密码

配置存储模式

```bash
git config --global credential.helper store
```

执行之后会在用户主目录下的.gitconfig文件中多加 helper = store

Linux 下查看：

```bash
vim ~/.gitconfig
```

windows10 下当前用户路径：`%USERPROFILE%`

内容如下：

```bash
[user]
        name = TangQuanwei
        email = 1076451802@qq.com
[credential]
        helper = store
```

## 生成 SSH 公钥

```bash
ssh-keygen -t ed25519 -C "Gitee SSH Key"
```

- -t key 类型
- -C 注释

```bash
ls ~/.ssh/
cat C:\Users\10764\.ssh\id_ed25519.pub
```
