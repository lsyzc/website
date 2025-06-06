---
title: Some Linux Commands 
date: 2025-05-22
lang: zh
type: note
---

这里记录一些平常用到的 Linux 命令

## 添加用户
可以选择 useradd 或者 adduser

useradd -m -s /bin/bash 用户名 # （-m 生成家目录 -s 指定默认 shell）

adduser 用户名


## 将用户赋予 sudo 权限
1. 将用户加入 sudo 组
```shell
sudo usermod -aG sudo 用户名
```
2. 确认sudoers配置

执行以下命令检查 /etc/sudoers 文件里有没有对 sudo 组的配置：

```shell
sudo cat /etc/sudoers | grep sudo
```

正常情况下，应该有一行类似：

```shell
%sudo   ALL=(ALL:ALL) ALL
```
这行表示 sudo 组的成员拥有sudo权限。

如果这行被注释了（前面有 #），请用管理员身份编辑 /etc/sudoers，去掉注释。

编辑sudoers文件建议用 visudo，防止语法错误导致无法使用sudo。命令如下
```shell
EDITOR=vim sudo visudo
```

