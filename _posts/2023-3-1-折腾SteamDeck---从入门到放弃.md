---
layout : article
title : "折腾SteamDeck---从入门到放弃"
---

### Part 1 ---安装SteamOS和Windows双系统

### Part ? ---常见问题

#### 1,windows更新后refind掉引导

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
