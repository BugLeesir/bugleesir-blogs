---
layout : article
title : "Windows批处理教程"
---

### 前言---为什么要写这个教程

本人是计算机专业的一名大学牲，一直以来对Windows的批处理程序很感兴趣，但是网上又没有很合适的教程可以进行学习，于是我就决定在我学习批处理的过程中编写一份教程，既是为了让自己忘了的时候能重新想起自己学了什么，也为了让更多的人了解Windows批处理这一强大的功能。

### 序言---何为批处理？

* 批处理(Batch)，也称为批处理脚本，扩展名为.bat或者.cmd。

* 类似于linux或Unix中的shell脚本。

* 包含一系列 DOS命令，通常用于自动执行重复性任务。只需我们双击批处理文件便可执行任务，而无需重复输入相同指令。

* 批处理文件可以极大程度地节省时间，在应对重复性工作时尤其有效，熟练使用可以简化很多重复工作，提高工作效率。

**[Microsoft官方命令指南](https://learn.microsoft.com/zh-cn/windows-server/administration/windows-commands/windows-commands)**

### Part1---初识批处理程序

```CMD
@echo off
echo "请输入需要移动的文件/文件夹的绝对路径"
set /P source=
echo "请输入转移目标位置的绝对路径"
set /P target=
cd %source%
xcopy * %target% /H /E /Y
for %%I in (%source%) do set temp=%%~nI
cd ..
rd/s/q %temp%
mklink /D "%source%" "%target%"
pause
@REM 作者BugLeesir
```

先看上面这个我编写的批处理程序，本教程将从这个程序出发带领大家初识Windows批处理

这个批处理程序实现了我博客中
**[mklink命令的使用](https://bugleesir.github.io/2022/10/20/mklink%E5%91%BD%E4%BB%A4%E7%9A%84%E4%BD%BF%E7%94%A8.html)**
中提到的符号链接的功能，可以将一些强制安装在C盘的流氓软件移动到别的磁盘，节省电脑的C盘空间。

### Part2---常用命令

* echo
  
官方介绍：显示消息或者打开或关闭命令回显功能。 如果不结合任何参数使用，echo 会显示当前回显设置。

简介：就是敲代码时候的print输出信息，若不在批处理文件第一行加上`@echo off`则每一行批处理中的命令都会被显示在控制台。

语法：

```CMD
echo [<message>]    //指定要在屏幕上显示的文本
echo [on | off]     // 打开或关闭命令回显功能。 命令回显功能默认已打开

```

* rem

官方介绍:在脚本、批处理或config.sys文件中记录注释。 如果未指定注释， rem 将添加垂直间距。

语法:

```CMD
@REM [<comment>]    //单行注释

REM/||(
    The REM statement evaluates to success,
    so these lines will never be executed.
    Keep in mind that you will need to escape closing parentheses
    within multi-line comment blocks like shown in this example. ^)
  )     //多行注释
```
