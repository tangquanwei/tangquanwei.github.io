---
title: RHEL RPM 软件包管理
date: 2023-05-18 10:33:35
tags: Linux
---

## RPM 命令
```bash

```

### 安装
```bash
rpm -ivh <rpm_package_name>
```
![](2023-05-18-10-39-54.png)

rpm命令不能自己处理依赖

所以换个没有其他依赖的

![](2023-05-18-10-46-24.png)

### 删除

```bash
rpm -e {<rpm_package_name>|<name>}
```

![](2023-05-18-10-41-18.png)

没有抛出异常说明执行成功

### 查询

```bash
rpm -qa | grep <name>           所有
rpm -q  <rpm_package_name>      指定
rpm -qp <rpm_package_file_name> 安装前了解
rpm -qi <rpm_package_name>      infomation
rpm -ql <rpm_package_name>      包含的文件
rpm -qf <file_name>             文件属于哪个包
rpm -qd <rpm_package_name>      文档
rpm -qc <rpm_package_name>      配置
rpm -qip <rpm_package_name>     安装前了解信息
```
![](2023-05-18-11-02-55.png)

![](2023-05-18-11-04-40.png)

![](2023-05-18-11-05-26.png)

![](2023-05-18-11-05-53.png)

![](2023-05-18-11-07-13.png)

![](2023-05-18-11-08-52.png)

![](2023-05-18-11-09-38.png)

eg. 通过rpm找出vimrc的注释符号
```bash
rpm -qc vim-common | xargs vim
```
![](2023-05-18-11-13-57.png)

不难看出"就是vim的注释符

### 升级

```bash
rpm -Uvh <rpm_package_name>
```

### 验证

```bash
rpm -V [args] {<rpm_package_name>|<name>}
```
对/etc/vimrc稍作修改
![](2023-05-18-11-16-17.png)

验证
![](2023-05-18-11-15-25.png)

查查man page
![](2023-05-18-11-18-14.png)