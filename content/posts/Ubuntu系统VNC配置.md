---
title:  'Ubuntu系统VNC配置'
date: 2023-11-27T14:17:59+08:00

---

## 安装VNC服务端

首先在服务器上安装tigerVNC，命令如下。（这里没有选择realVNC是因为其在多用户访问时出现问题）

```bash
sudo apt install tigervnc-standalone-server tigervnc-xorg-extension
```

## 配置VNC

1. 首先管理员编辑用户配置文件

```bash
vim /etc/tigervnc/vncserver.users
```

将端口号与用户名对应，如用户andrew使用5902端口，用户lisa使用5903端口的配置如下：

```mipsasm
:2=andrew
:3=lisa
```

1. 给每个用户配置config文件

以为andrew配置为例，先使用用户andrew执行命令`vncserver`,再执行`vncserver -kill :*`关闭刚启动的VNC桌面，这样VNC的配置目录（~/.vnc）就建立好了。
接着新建文件/home/andrew/.vnc/config，内容如下：

```ini
session=ubuntu
geometry=1920x1080
securitytypes=vncauth,tlsvnc
```

## 管理员启动VNC桌面

例如为5902端口的andrew开启VNC，运行命令

```bash
systemctl start tigervncserver@:2
```

设置为开机启动：

```bash
systemctl enabletigervncserver@:2
```

查看端口状态：

```bash
netstat -tunlp | grep vnc
```

如果看到5902则说明VNC正确运行。如果没看到则可以多尝试几次systemctl start。
可以使用如下命令查看启动信息：

```bash
journalctl -xeu tigervncserver@:2
```
