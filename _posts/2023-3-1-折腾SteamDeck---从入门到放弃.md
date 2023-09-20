---
layout : article
title : "折腾SteamDeck---从入门到放弃"
---

### Part 1 ---安装SteamOS和Windows双系统

### Part ? ---常见问题

#### 1、windows更新后refind掉引导

windows bat脚本，粘贴在txt文件中改后缀为.bat双击运行即可

```shell
PUSHD %~DP0 & cd /d "%~dp0"
%1 %2
mshta vbscript:createobject("shell.application").shellexecute("%~s0","goto :runas","","runas",1)(window.close)&goto :eof
:runas
@echo off
set /p path=请输入refind_x64.efi文件的路径
bcdedit /set {bootmgr} path %path%
```

#### 2、使用 ToMoon 永久解决 Steam Deck 网络问题

由于Steam在中国没有部署服务器，所以我们如果想要连接上Steam商店则需要靠魔法

##### 安装

打开到 Steam Deck 设置界面

系统 -> 系统设置 -> 打开开发者模式

回到设置向下翻，找到开发者 -> 打开 CEF 远程调试

等待 Steam Deck 重启

按电源键切换到 Desktop 桌面模式

打开 Konsole，输入`curl -L http://dl.ohmydeck.net | sh`安装 Plugin Loader

输入`curl -L http://i.ohmydeck.net | sh`安装 Tomoon

切换回到 Gamming 游戏模式，按下右侧摇杆下的快捷按钮（三个点的按钮），可以看到多了一个 Decky 插件面板

##### 使用

打开 Manage Subscriptions，添加你服务商提供的 Clash 订阅链接并下载

下载完成后，切换回主界面选择订阅并点击启动

在桌面模式可通过浏览器 <http://127.0.0.1:9090/ui> 打开仪表盘

注意：若是订阅链接过长可以使用短域名缩短服务，如 t.ly n9.cl.

别忘了在缩短后的链接前面加 http(s)://，形如 <https://n9.cl/abcdef> 才是有效的订阅链接
