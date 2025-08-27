---
layout : article
title : sunshine + moonlight + frp +公网服务器实现远程串流
---

<!-- TOC -->

- [sunshine](#sunshine)
  - [简述](#简述)
  - [安装步骤](#安装步骤)

<!-- /TOC -->

## sunshine

### 简述

Sunshine 是一个开源的游戏串流服务器，支持 Windows、Linux 等平台。它可以将本地电脑的桌面或游戏画面编码后，通过网络实时推送到客户端。Sunshine 支持 NVIDIA NVENC、AMD VCE、Intel QSV 等硬件加速编码，延迟低，性能高。

### 安装步骤

- 1、作为被控端，前往[sunshine官网](https://app.lizardbyte.dev/Sunshine/?lng=zh-CN)进行下载
- 2、进入sunshine后需要先设置账号和密码，设置完成后显示以下画面：

![sunshine登陆](https://raw.githubusercontent.com/BugLeesir/image_host01/main/blogs_img/20250827090335.png)

![sunshine主界面](https://raw.githubusercontent.com/BugLeesir/image_host01/main/blogs_img/20250827092007.png)

- 3、此后直接登陆即可，需要串流时需保持后台启动该程序
