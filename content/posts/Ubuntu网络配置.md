---
title: "Ubuntu网络配置"
date: 2023-11-27T13:43:28+08:00


---

# Ubuntu网络环境配置

## IP

### ip命令



1. **查看网络接口名称**： 运行以下命令查看网络接口名称：

   ```bash
   ip a
   ```

   找到想配置IP地址的网络接口名称，通常以 `eth0`、`eth1`、`enp0s3` 等命名。

2. **配置IP地址**： 假设要配置 `eth0` 接口的IP地址为 `192.168.1.100`，子网掩码为 `255.255.255.0`：

   ```bash
   sudo ip addr add 192.168.1.100/24 dev eth0
   ```

   这会将IP地址设置为 `192.168.1.100`，子网掩码为 `/24`（相当于 `255.255.255.0`），并将其分配给 `eth0` 接口。

3. **激活网络接口**： 如果配置后仍未激活接口，可以运行以下命令：

   ```bash
   sudo ip link set eth0 up
   ```

### netplan命令

1. **网络配置文件**： 使用文本编辑器打开网络配置文件，比如：

   ```bash
   sudo nano /etc/netplan/00-installer-config.yaml
   ```

   这里的文件名可能因配置而异。

2. **编辑配置文件**： 在文件中找到对应的接口，可能看起来类似于：

   ```yaml
   network:
     ethernets:
       eth0:
         addresses: [192.168.1.100/24]
         gateway4: 192.168.1.1
         nameservers:
           addresses: [8.8.8.8, 8.8.4.4]
   ```

   修改 `addresses` 下的IP地址和子网掩码，然后保存文件。

3. **应用更改**： 应用更改并重启网络服务：

   ```bash
   sudo netplan apply
   sudo systemctl restart network-manager
   ```

## DNS

如果通过netplan，则已经配置好dns，如果使用ip命令，则需要如下操作

```bash
sudo vim /etc/resolv.conf
```

往后追加如下内容

```
nameserver 114.114.114.114
```

