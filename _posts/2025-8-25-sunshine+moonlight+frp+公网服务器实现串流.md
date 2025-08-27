---
layout : article
title : sunshine + moonlight + frp +公网服务器实现远程串流
---

<!-- TOC -->

- [Sunshine](#sunshine)
  - [简述](#简述)
  - [安装步骤](#安装步骤)
- [Fast Reverse Proxy(frp)](#fast-reverse-proxyfrp)
  - [简述](#简述-1)
  - [安装步骤（服务端）](#安装步骤服务端)
  - [安装步骤（客户端）](#安装步骤客户端)
- [Moonlight](#moonlight)
  - [简述](#简述-2)
  - [安装步骤](#安装步骤-1)

<!-- /TOC -->

## Sunshine

### 简述

Sunshine 是一个开源的游戏串流服务器，支持 Windows、Linux 等平台。它可以将本地电脑的桌面或游戏画面编码后，通过网络实时推送到客户端。Sunshine 支持 NVIDIA NVENC、AMD VCE、Intel QSV 等硬件加速编码，延迟低，性能高。

### 安装步骤

- 1、作为被控端，前往[sunshine官网](https://app.lizardbyte.dev/Sunshine/?lng=zh-CN)进行下载
- 2、进入sunshine后需要先设置账号和密码，设置完成后显示以下画面：

![sunshine登陆](https://raw.githubusercontent.com/BugLeesir/image_host01/main/blogs_img/20250827090335.png)

![sunshine主界面](https://raw.githubusercontent.com/BugLeesir/image_host01/main/blogs_img/20250827092007.png)

- 3、此后直接登陆即可，需要串流时需保持后台启动该程序

## Fast Reverse Proxy(frp)

### 简述

frp 是一个高性能的反向代理应用，主要用于内网穿透。通过 frp，你可以将内网的 Sunshine 服务端口映射到公网服务器上，实现外网设备访问内网服务。frp 包含 frps（服务端，部署在公网服务器）和 frpc（客户端，部署在内网机器）。

### 安装步骤（服务端）

- 1、前往Github的[frp仓库](https://github.com/fatedier/frp/tree/dev)拉取对应操作系统的程序并解压
- 2、前往服务器防火墙，将以下端口放通：

![frp放行端口](https://raw.githubusercontent.com/BugLeesir/image_host01/main/blogs_img/20250827125811.png)

- 3、编写批处理脚本并运行，每次需要启用服务的时候就运行脚本即可(若服务器有安全软件需要将frps列入白名单)

```batch
@echo off
cd /d C:\Users\Administrator\frp  // 这里是你实际存放frps.exe的目录
frps -c frps.toml
pause
```

### 安装步骤（客户端）

- 1、前往Github的[frp仓库](https://github.com/fatedier/frp/tree/dev)拉取对应操作系统的程序并解压
- 2、编写frpc.toml文件，根据服务需要进行编写需要的端口配置，串流所需要的端口如下：

tcp:47984.47989.48010.47998.46662
udp:47998.47999.48000.48002.48010.46662

```toml
serverAddr = "39.108.214.39"  // 这里是你的公网服务器地址
serverPort = 7000

[[proxies]]
name = "tcp-47984"
type = "tcp"
localIP = "127.0.0.1"
localPort = 47984
remotePort = 47984

[[proxies]]
name = "tcp-47989"
type = "tcp"
localIP = "127.0.0.1"
localPort = 47989
remotePort = 47989

[[proxies]]
name = "tcp-48010"
type = "tcp"
localIP = "127.0.0.1"
localPort = 48010
remotePort = 48010

[[proxies]]
name = "udp-47998"
type = "udp"
localIP = "127.0.0.1"
localPort = 47998
remotePort = 47998

[[proxies]]
name = "udp-47999"
type = "udp"
localIP = "127.0.0.1"
localPort = 47999
remotePort = 47999

[[proxies]]
name = "udp-48000"
type = "udp"
localIP = "127.0.0.1"
localPort = 48000
remotePort = 48000

[[proxies]]
name = "udp-48002"
type = "udp"
localIP = "127.0.0.1"
localPort = 48002
remotePort = 48002

[[proxies]]
name = "udp-48010"
type = "udp"
localIP = "127.0.0.1"
localPort = 48010
remotePort = 48010

[[proxies]]
name = "test-27036-tcp"
type = "tcp"
localIP = "127.0.0.1"
localPort = 27036
remotePort = 27036

[[proxies]]
name = "test-27036-udp"
type = "udp"
localIP = "127.0.0.1"
localPort = 27036
remotePort = 27036

[[proxies]]
name = "test-27037-tcp"
type = "tcp"
localIP = "127.0.0.1"
localPort = 27037
remotePort = 27037

[[proxies]]
name = "test-27031-udp"
type = "udp"
localIP = "127.0.0.1"
localPort = 27031
remotePort = 27031
```

- 3、编写批处理脚本并运行，每次需要启用服务的时候就运行脚本即可

```batch
@echo off
cd /d E:\AppDate\frp // 这里是你实际存放frpc.exe的目录
.\frpc -c frpc.toml
pause
```

## Moonlight

### 简述

Moonlight 是一个开源的串流客户端，最初是为 NVIDIA GameStream 设计的，但现在也支持 Sunshine。Moonlight 可以在多种设备（如 Windows、Android、iOS、树莓派等）上运行，接收 Sunshine 推送的画面，实现远程控制和游戏。

### 安装步骤

- 1、前往[moonlight官网](https://moonlight-stream.org/)下载对应操作系统的安装包
- 2、输入公网服务器地址进行连接
- 3、被控端输入匹配码进行匹配即
