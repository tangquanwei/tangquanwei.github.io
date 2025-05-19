---
title: Arch linux 
date: 2022-09-24 15:47:12
tags: Linux
cover: arch.png
---


## 安装软件

  使用 pacman 安装、删除、升级

* 安装指定的包
```bash
  pacman -S <包名_1> <包名_2> ...
```
* 安装一个本地包(不从源里下载）
```bash
  pacman -U /path/to/package/package_name-version.pkg.tar.zst
```
* 安装一个远程包（不在 pacman 配置的源里面）
```bash
  pacman -U <http://www.example.com/repo/example.pkg.tar.zst>
```
* 删除单个软件包，保留其全部已经安装的依赖关系
```bash
  pacman -R package_name
```
* 删除指定软件包，及其所有没有被其他已安装软件包使用的依赖关系
```bash
  pacman -Rs package_name
```
* 上面这条命令在移除包含其他所需包的组时有时候会拒绝运行。这种情况下可以尝试
```bash
  pacman -Rsu package_name
```
* 升级所有软件包
```bash
  pacman -Syu
```
* 查询包数据库
```bash
  pacman 使用 -Q 参数查询本地软件包数据库， -S 查询同步数据库，以及 -F查询文件数据库
```
* pacman 可以在包数据库中查询软件包，查询位置包含了软件包的名字和描述
```bash
  pacman -Ss string1 string2 ...
```
* 要查询已安装的软件包
```bash
  pacman -Qs string1 string2 ...
```
* 按文件名查找软件库
```bash
  pacman -F string1 string2 ...
```
* 显示软件包的详尽的信息
```bash
  pacman -Si package_name
```
* 使用两个 -i 将同时显示备份文件和修改状态
```bash
  pacman -Qii package_name
```
* 要获取已安装软件包所包含文件的列表
```bash
  pacman -Ql package_name
```
* 要罗列所有不再作为依赖的软件包(孤立orphans)
```bash
  pacman -Qdt
```
* 清理软件包缓存
```bash
  pacman 将下载的软件包保存在 /var/cache/pacman/pkg/ 并且不会自动移除旧的和未安装版本的软件包
```
* 删除所有缓存的版本和已卸载的软件包，除了最近的3个会被保留
```bash
  paccache -r
```

## 更新系统

日常更新

```bash
sudo pacman -Syu 
```
或者

```bash
yay -Syu
```

<!-- more -->

手头上保留 Arch 安装盘有问题时可以进行修正

旧配置文件

  ~/.config/        – 软件保存配置文件的地方
  ~/.cache/         – 程序缓存大小可能持续增加
  ~/.local/share/   – 可能有旧文件

手动处理

  warning: /etc/pam.d/usermod installed as /etc/pam.d/usermod.pacnew
  warning: /etc/pam.d/usermod saved as /etc/pam.d/usermod.pacsave

### 网络

arp ->	ip neighbor
ifconfig ->	ip address, ip link
netstat ->	ss
route ->	ip route 

### 切换 Java 版本

```                        
archlinux-java <COMMAND>

COMMAND:
	status		List installed Java environments and enabled one
	get		Return the short name of the Java environment set as default
	set <JAVA_ENV>	Force <JAVA_ENV> as default
	unset		Unset current default Java environment
	fix		Fix an invalid/broken default Java environment configuration
```

切换Java版本
```bash
sudo archlinux-java set java-17-openjdk
```

### 解压 .zip 文件乱码

原因：

由于zip格式中并没有指定编码格式，Windows下生成的zip文件中的编码是GBK/GB2312等。
因此，导致这些zip文件在Linux下解压时出现乱码问题，因为Linux下的默认编码是UTF8。

解决：

安装修改版的 unzip
```bash
yay -S unzip-natspec 
```

解压时指定
```bash
unzip -O cp936 <filename>.zip
```

## 双显示器

置外接显示器检测

将参数nvidia-drm.modeset=1加入到/etc/default/grub文件中的GRUB_CMDLINE_LINUX_DEFAULT末尾，如下，命令一定不要输错了，不然无法开机，如果出现无法开机的情况，请用启动盘启动将修改的文件还原。

```
GRUB_CMDLINE_LINUX_DEFAULT="quiet apparmor=1 security=apparmor udev.log_priority=3 nvidia-drm.modeset=1"
```
命令行输入(系统管理员模式)
```bash
grub-mkconfig > /boot/grub/grub.cfg
```
（重启生效）