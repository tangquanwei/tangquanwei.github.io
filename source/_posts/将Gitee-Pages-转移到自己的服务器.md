---
title: 将Gitee Pages 转移到自己的服务器
date: 2023-05-22 19:18:42
tags: 
- Linux
- Git
---

# 将Gitee Pages 转移到自己的服务器

## 开始之前

我在自己的电脑上用 hexo 框架配置了一个博客。本来一切安好直到最近换了个主题博客访问变得有点慢了（原因应该是js比较多。。。）但是我不想放弃现在的主题，所以就决定转移原来部署在Gitee Pages的博客到自己租的服务器（是的，我有服务器。但是只有一年，用完了还得倒数据，嫌麻烦。。。）

我发现网页所有的资源都在这个文件夹下

![](2023-05-22-19-24-53.png)

而这个文件夹被推送到了gitee

现在就好办了，直接从gitee克隆到自己的服务器就行了。

## 克隆

复制这个链接

![](2023-05-22-19-27-41.png)

在服务器上找个合适的地方执行
```bash
quanwei@VM-8-7-ubuntu:~/workplaceFolder$ git clone https://gitee.com/quanw20/quanw20.git
```

输入密码（因为是私有的）就克隆下来了

![](2023-05-22-19-30-02.png)


## 更新

每次推送到gitee之后服务器上的数据都不会自动更新（不知道有没有什么hook之类的）

所以我会在每次更新之后向服务器发送一条命令用来拉取更新,吧结果写入log

```bash
ssh name@host "cd /dir/quanw20;git pull > ./update.log;date >> ./update.log"
```

但是的但是ssh需要输密码就很不优雅,不过ssh有另一种用密钥对来验证的方式：  

就是把本地机的公钥放到服务器

本地机：
```bash
ssh-keygen -t rsa

ls ~/.ssh

cat ~/.ssh/*.pub #复制输出
```

服务器：
```bash
vim ~/.ssh/authorized_keys # 这个文件要有
# 将上面复制的内容粘贴到文件里面
```

命令有点长也可以alias取别名

vim ~/.zshrc

```bash
alias update_blog='ssh name@host "cd /dir/quanw20;git pull > ./update.log;date >> ./update.log"'
```
![](2023-05-22-19-50-32.png)

![](2023-05-22-19-50-03.png)

OK！ 大功告成

吃饭去 ~ 
