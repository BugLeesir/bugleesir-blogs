---
layout : article
title : "Windows批处理教程"
---

- [前言—为什么要写这个教程](#前言-为什么要写这个教程)
- [序言—何为批处理？](#序言-何为批处理)
- [Part1—初识批处理程序](#part1-初识批处理程序)
- [Part2—常用命令](#part2-常用命令)
- [Part3—变量](#part3-变量)
- [Part4—符号拓展](#part4-符号拓展)
- [Part5—常用特殊符号](#part5-常用特殊符号)
- [Part6—常用技巧](#part6-常用技巧)

### 前言-为什么要写这个教程

本人是计算机专业的一名大学牲，一直以来对Windows的批处理程序很感兴趣，但是网上又没有很合适的教程可以进行学习，于是我就决定在我学习批处理的过程中编写一份教程，既是为了让自己忘了的时候能重新想起自己学了什么，也为了让更多的人了解Windows批处理这一强大的功能。

### 序言-何为批处理？

- 批处理(Batch)，也称为批处理脚本，扩展名为.bat或者.cmd。

- 类似于linux或Unix中的shell脚本。

- 包含一系列 DOS命令，通常用于自动执行重复性任务。只需我们双击批处理文件便可执行任务，而无需重复输入相同指令。

- 批处理文件可以极大程度地节省时间，在应对重复性工作时尤其有效，熟练使用可以简化很多重复工作，提高工作效率。

**[Microsoft官方命令指南](https://learn.microsoft.com/zh-cn/windows-server/administration/windows-commands/windows-commands)**

### Part1-初识批处理程序

```bat
@echo off
setlocal enabledelayedexpansion
chcp 65001
echo "请输入需要移动的文件/文件夹的绝对路径"
set /P source=
echo "请输入转移目标位置的绝对路径"
set /P target=
if exist %source%\ (
for %%I in (%source%) do set temp=%%~nI
echo !temp!
echo %target%\!temp!
md %target%\!temp!
xcopy %source% %target%\!temp! /H /E /Y
rd %source% /s/q
mklink /D "%source%" "%target%\!temp!"
pause
) else (
for %%I in (%source%) do set temp=%%~nxI
copy %source% %target%
del %source%
mklink %source% %target%\!temp!
pause
)
@REM 作者BugLeesir
```

先看上面这个我编写的批处理程序，本教程将从这个程序出发带领大家初识Windows批处理

这个批处理程序实现了我博客中
**[mklink命令的使用](https://bugleesir.github.io/bugleesir-blogs/2022/10/20/mklink%E5%91%BD%E4%BB%A4%E7%9A%84%E4%BD%BF%E7%94%A8.html)**
中提到的符号链接的功能，可以将一些强制安装在C盘的流氓软件移动到别的磁盘，节省电脑的C盘空间。

### Part2-常用命令

- (command)/?

官方介绍：在 cmd 窗口中显示帮助文本。

语法：

```shell
C:\Users\10078>echo /?
显示消息，或者启用或关闭命令回显。

  ECHO [ON | OFF]
  ECHO [message]

若要显示当前回显设置，请键入不带参数的 ECHO。
```

- echo
  
官方介绍：显示消息或者打开或关闭命令回显功能。 如果不结合任何参数使用，echo 会显示当前回显设置。

简介：就是敲代码时候的print输出信息，若不在批处理文件第一行加上`@echo off`则每一行批处理中的命令都会被显示在控制台。

语法：

```bat
echo [<message>]    //指定要在屏幕上显示的文本
echo [on | off]     // 打开或关闭命令回显功能。 命令回显功能默认已打开

```

- rem

官方介绍:在脚本、批处理或config.sys文件中记录注释。 如果未指定注释， rem 将添加垂直间距。

语法:

```bat
@REM [<comment>]    //单行注释

REM/||(
    The REM statement evaluates to success,
    so these lines will never be executed.
    Keep in mind that you will need to escape closing parentheses
    within multi-line comment blocks like shown in this example. ^)
  )     //多行注释
```

- move

官方介绍:将一个或多个文件从一个目录移到另一个目录。将加密文件移动到不支持加密文件系统的卷 (EFS) 结果将导致错误。 必须先解密文件或将其移动到支持 EFS 的卷。

语法：

```bat
move [{/y|-y}] [<source>] [<target>]
/Y 停止提示确认要覆盖现有目标文件。 此参数可能在 COPYCMD 环境变量中预设。 可以使用 -y 参数替代此预设。 除非命令是从批处理脚本中运行，否则默认设置是在覆盖文件之前进行提示。
-y开始提示确认是否要覆盖现有目标文件。
<source>指定要移动 () 的文件的路径和名称。 若要移动或重命名目录， 源 应为当前目录路径和名称。
<target>指定要将文件移动到的路径和名称。 若要移动或重命名目录， 目标 应为所需的目录路径和名称。
```

- del/erase

 官方介绍：删除一个或多个**文件**。如果使用 del 从磁盘中删除文件，则无法检索该文件。del 命令还可以使用不同的参数从 Windows 恢复控制台运行。

 简介:删除文件的命令而不是文件夹。

 语法:

```bat
del [/p] [/f] [/s] [/q] [/a[:]<attributes>] <names>
erase [/p] [/f] [/s] [/q] [/a[:]<attributes>] <names>
<names>指定一个或多个文件或目录的列表。 通配符可用于删除多个文件。 如果指定了目录，则将删除该目录中的所有文件。
/p提示在删除指定文件之前进行确认。
/f强制删除只读文件。
/s从当前目录和所有子目录中删除指定的文件。 在文件被删除时显示文件的名称。
/q指定静默模式。 系统不会提示你进行删除确认。
/a[：]<attributes>根据以下文件属性删除文件：
r 只读文件
h 隐藏的文件
i 不是内容索引文件
s 系统文件
准备 存档的文件
l 重分析点
- 用作前缀，表示“not”

可以使用通配符 (* 和 ？) 一次删除多个文件。 但是，为了避免无意中删除文件，应谨慎使用通配符。
del *.*
```

- rd/rmdir

官方介绍：删除目录。rd 命令还可以使用不同的参数从 Windows 恢复控制台运行。

简介：用来删除文件夹的命令。

语法：

``` bat
rd [<drive>:]<path> [/s [/q]]

[<drive>:]<path>指定要删除的目录的位置和名称。 路径 是必需的。 如果在指定 路径的开头包含反斜杠 () ，则无论当前目录) 如何，该 路径 都从根目录 (开始。
/s删除指定目录及其所有子目录 (目录树，包括) 的所有文件。
/q指定静默模式。 删除目录树时不提示确认。 仅当还指定了 /s 时，/q 参数才有效。
谨慎： 在静默模式下运行时，无需确认即可删除整个目录树。 在使用 /q 命令行选项之前，请确保移动或备份重要文件。

不能删除包含文件（包括隐藏文件或系统文件）的目录。 如果尝试这样做，将显示以下消息：

The directory is not empty

使用 dir /a 命令列出所有文件 (包括隐藏文件和系统文件) 。 然后结合 -h 使用 attrib 命令删除隐藏的文件属性，使用 -s 删除系统文件属性，或使用 -h -s 删除隐藏和系统文件属性。 删除隐藏和文件属性后，可以删除这些文件。

不能使用 rd 命令删除当前目录。 如果尝试删除当前目录，将显示以下错误消息：

The process can't access the file because it is being used by another process.

如果收到此错误消息，则必须更改为其他目录 (不是当前目录) 的子目录，然后重试。
```

- attrib

官方介绍:显示、设置或删除分配给文件或目录的属性。 如果在不使用参数的情况下使用， 则 attrib 会显示当前目录中所有文件的属性。

简介:用来消除文件的属性，有时由于文件的只读属性无法删除移动时则需要先消除文件属性。

语法：

```bat
attrib [{+|-}r] [{+|-}a] [{+|-}s] [{+|-}h] [{+|-}i] [<drive>:][<path>][<filename>] [/s [/d] [/l]]

{+|-}r设置 (+) 或清除 (-) 只读文件属性。
{+\|-}a设置 (+) 或清除 (-) 存档文件属性。 此属性集标记自上次备份以来已更改的文件。 请注意， xcopy 命令使用存档属性。
{+\|-}s设置 (+) 或清除 (-) 系统文件属性。 如果文件使用此属性集，则必须先清除该属性，然后才能更改该文件的任何其他属性。
{+\|-}h) + Hidden 文件属性设置 () 或清除 - (。 如果文件使用此属性集，则必须先清除该属性，然后才能更改该文件的任何其他属性。
{+\|-}i设置 (+) 或清除 (-) 非内容索引文件属性。
[<drive>:][<path>][<filename>]指定要显示或更改其属性的目录、文件或文件组的位置和名称。
可以在 filename 参数中使用 ？ 和*通配符来显示或更改一组文件的属性。

/s将 attrib 和任何命令行选项应用于当前目录及其所有子目录中的匹配文件。
/d将 attrib 和任何命令行选项应用于目录。
/l将 attrib 和任何命令行选项应用于符号链接，而不是符号链接的目标。
```

- goto

官方介绍:将 cmd.exe 定向到批处理程序中的标记行。 在批处理程序中，此命令将命令处理定向到由标签标识的行。 找到标签后，继续处理，从下一行开始的命令开始。

简介：类似于C语言中的goto语句

语法:

```bat
goto <label>

<label>指定在批处理程序中用作标签的文本字符串。
/?在命令提示符下显示帮助。

可以在 label 参数中使用空格，但不能包含其他分隔符（例如，分号 (;) 或等号 (=)）。

为 label 指定的值必须与批处理程序中的标签匹配。 批处理程序中的标签必须以冒号 (:) 开头。 如果某行以冒号开头，则系统会将此行视为标签，并忽略此行中的任何命令。 如果批处理程序不包含你在 label 参数中指定的标签，则批处理程序将停止并显示以下消息：Label not found。

```

- call

官方介绍：从另一个批处理程序调用一个批处理程序，而不停止父批处理程序。 call 命令接受标签作为调用的目标。

备注：在脚本或批处理文件外部使用 call 时，它在命令提示符下不起作用。

语法:

```bat
call [drive:][path]<filename> [<batchparameters>]] 
call [:<label> [<arguments>]]

[<drive>:][<path>]<filename>指定要调用的批处理程序的位置和名称。 参数 <filename> 是必需的，并且它必须具有 .bat 或 .cmd 扩展名。
<batchparameters>指定批处理程序所需的任何命令行信息。
:<label>指定希望批处理程序控件跳转到的标签。
<arguments>指定要传递到批处理程序的新实例的命令行信息（从 :<label> 开始）。
/?在命令提示符下显示帮助。

使用批处理参数：

批处理参数可以包含可以传递给批处理程序的任何信息，包括命令行选项、文件名、批处理参数 %0 到 %9 以及变量（例如 %baud%）。

使用 <label> 参数：

通过将 call 与 <label> 参数一起使用，可以创建新的批处理文件上下文，并将控件传递给指定标签后的语句。 第一次遇到批处理文件的结尾时（即跳转到标签后），控件会返回到 call 语句后的语句。 第二次遇到批处理文件的结尾时，会退出批处理脚本。

使用管道和重定向符号：

请勿将管道 (|) 或重定向符号（< 或 >）与 call 一起使用。

进行递归调用

可以创建一个调用自身的批处理程序。 但是，必须提供退出条件。 否则，父批处理程序和子批处理程序可能会无限循环。

使用命令扩展

如果启用了命令扩展，call 将接受 <label> 作为调用的目标。 正确的语法为 call :<label> <arguments>。
```

### Part3-变量

- set

官方介绍：显示、设置或删除cmd.exe环境变量。 如果在不使用参数的情况下使用， 则 set 将显示当前环境变量设置。此命令需要默认启用的命令扩展。set 命令还可以使用不同的参数从 Windows 恢复控制台运行。

简介：set命令可以用来获取，设置变量，所以可以运用这个命令在批处理脚本中使用变量。

语法：

```bat
set [<variable>=[<string>]]
set [/p] <variable>=[<promptString>]
set /a <variable>=<expression>

<variable>指定要设置或修改的环境变量。
<string>指定要与指定的环境变量关联的字符串。
/p将 的值 <variable> 设置为用户输入的输入行。
<promptstring>指定提示用户输入的消息。 此参数必须与 /p 参数一起使用。
/a将 设置为 <string> 计算的数字表达式。
<expression>指定数值表达式。
```

#### 用法1---不带参数的set，获取环境变量

```shell
C:\Users\10078>set java_home
JAVA_HOME=D:\tools\Java\jdk-18.0.1.1
```

#### 用法2---使用‘=’为变量赋值

```shell
C:\Users\10078>set temp_var="hihao"
C:\Users\10078>echo %temp_var%
"hihao"
```

#### 用法3---使用带'/a'参数的set命令，执行数字表达式并赋值给变量

```shell
C:\Users\10078>set /a temp_var=11*14
154
C:\Users\10078>echo %temp_var%
154
```

![error](https://raw.githubusercontent.com/BugLeesir/image_host01/main/blogs_img/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202023-03-10%20164539.gif)

#### 用法4---适用带'/p'参数的set命令，变量将通过用户输入接收值，提示作为提示信息输出

```shell
C:\Users\10078>set /p test=请输入信息
请输入信息777
C:\Users\10078>echo %test%
777
```

#### 用法5---%var:str1=str2%表示将变量的值中包含的str1使用str2替换后获得的变量（原变量不发生变化）

```shell
C:\Users\10078>set test="hello my best world !"
C:\Users\10078>echo %test%
"hello my best world !"
C:\Users\10078>echo %test:best=beautiful%
"hello my beautiful world !"
C:\Users\10078>echo %test:hello=beautiful%
"beautiful my best world !"
C:\Users\10078>set test2=%test:best=beautiful%
C:\Users\10078>echo %test2%
"hello my beautiful world !"
```

#### 用法6---%var:~start[,length]%表示从start出开始（包括start，第一个计数为0)，取length长的子串，如果length省略，则表示取到串尾，start可以为负数，最后一个字符为-1，从后往前依次为-2、-3、-4……

```shell
C:\Users\10078>set test="hello world"
C:\Users\10078>echo %test:~2,5%
ello
C:\Users\10078>echo %test:~2%
ello world"
C:\Users\10078>echo %test:~-3%
ld"
```

#### 用法7---for ```<parameter>``` %i/%%i in (command1) do (command2) ,for循环

在文件（bat或者cmd）中需要将%i使用%%i来代替，%%i不会出现变量扩展的问题，在循环中同步更新%%i的值

for、in和do是for语句的关键字，它们三个缺一不可；

parameter是for语句的参数，有'/d','/r','/l','/f',也可以不使用该参数

%%I是for语句中对形式变量的引用，即使变量l在do后的语句中没有参与语句的执行，也是必须出现的；

in之后，do之前的括号不能省略；

command1表示字符串或变量，command2表示字符串、变量或命令语句；

##### 1，不带参数的for语句

```shell
C:\Users\10078>set test="hello world batch"
C:\Users\10078>for %I in (%test%) do echo %I
C:\Users\10078>echo "hello world batch"
"hello world batch"
C:\Users\10078>set test2="What a beautiful world!"
C:\Users\10078>set test3="sword new new"
C:\Users\10078>for %I in (%test%,%test2%,%test3%) do echo %I
C:\Users\10078>echo "hello world batch"
"hello world batch"
C:\Users\10078>echo "What a beautiful world!"
"What a beautiful world!"
C:\Users\10078>echo "sword new new"
"sword new new"
```

##### 2，带参数'/d'的for语句---处理文件夹

遍历目录

```shell
D:\MyCodes\test>tree
卷 wnist 的文件夹 PATH 列表
卷序列号为 74F6-FE76
D:.
├─testdir01
│  └─test01dir
├─testdir02
└─testdir03
    └─test03dir
        └─hahah

D:\MyCodes\test>for /d %i in (*) do (echo %i)

D:\MyCodes\test>(echo testdir01 )
testdir01

D:\MyCodes\test>(echo testdir02 )
testdir02

D:\MyCodes\test>(echo testdir03 )
testdir03

D:\MyCodes\test>for /d /r %i in (*) do (echo %i)       // '/d'与'/r'联合使用遍历所有文件夹

D:\MyCodes\test>(echo D:\MyCodes\test\testdir01 )
D:\MyCodes\test\testdir01

D:\MyCodes\test>(echo D:\MyCodes\test\testdir02 )
D:\MyCodes\test\testdir02

D:\MyCodes\test>(echo D:\MyCodes\test\testdir03 )
D:\MyCodes\test\testdir03

D:\MyCodes\test>(echo D:\MyCodes\test\testdir01\test01dir )
D:\MyCodes\test\testdir01\test01dir

D:\MyCodes\test>(echo D:\MyCodes\test\testdir03\test03dir )
D:\MyCodes\test\testdir03\test03dir

D:\MyCodes\test>(echo D:\MyCodes\test\testdir03\test03dir\hahah )
D:\MyCodes\test\testdir03\test03dir\hahah

```

#### 用法8---setlocal [Enable|Disable]DelayedExpansion

### Part4-符号拓展

扩充变量语法详解：

```bat
选项语法 :
~I - 删除任何引号 ，扩充 %I
%~fI - 将 %I 扩充到一个完全合格的路径名
%~dI - 仅将 %I 扩充到一个驱动器号
%~pI - 仅将 %I 扩充到一个路径
%~nI - 仅将 %I 扩充到一个文件名
%~xI - 仅将 %I 扩充到一个文件扩展名
%~sI - 扩充的路径只含有短名
%~aI - 将 %I 扩充到文件的文件属性
%~tI - 将 %I 扩充到文件的日期 / 时间
%~zI - 将 %I 扩充到文件的大小
%~$PATH:I - 查找列在路径环境变量的目录，并将 %I 扩充
            到找到的第一个完全合格的名称。如果环境变量名
            未被定义，或者没有找到文件，此组合键会扩充到
            空字符串
```

注：若要获取本批处理文件的路径作符号拓展则将%I替换为%0,若在CMD的for循环中则为%I,批处理中的for循环则为%%I

可以组合修饰符来得到多重结果 :

```bat
C:\Users\10078>set /p var1=
C:\Users\10078\Desktop\MySQL.lnk

C:\Users\10078>echo %var1%
C:\Users\10078\Desktop\MySQL.lnk

C:\Users\10078>for %I in (%var1%) do echo %~dpI         //拓展到驱动器号加路径

C:\Users\10078>echo C:\Users\10078\Desktop\
C:\Users\10078\Desktop\

C:\Users\10078>for %I in (%var1%) do echo %~nxI         //拓展到文件名和拓展名

C:\Users\10078>echo MySQL.lnk
MySQL.lnk

```

### Part5-常用特殊符号

- @ 命令行回显屏蔽符号

作用：关闭当前行的回显，ECHO OFF可以关闭掉整个批处理命令的回显，但不能关掉ECHO OFF这个命令，我们在ECHO OFF这个命令前加个@，就可以达到所有命令均不回显的要求

- ``>``重定向符

作用：输出重定向命令,这个字符的意思是传递并且覆盖，他所起的作用是将运行的结果传递到后面的范围（后边可以是文件，也可以是默认的系统控制台）
在NT系列命令行中，重定向的作用范围由整个命令行转变为单个命令语句，受到了命令分隔符,&&,||和语句块的制约限制。

语法:

```bat
echo hello >1.txt rem:将建立文件1.txt，内容为”hello “（注意行尾有一空格）
echo hello>1.txt  rem:将建立文件1.txt，内容为”hello“（注意行尾没有空格）

//最后得到的txt文件中为行尾没有空格的hello
```

- ``>>``重定向符

作用：输出重定向命令这个符号的作用和>类似，但他们的区别是>>是传递并在文件的末尾追加，而>是覆盖

语法：

```bat
echo hello > 1.txt
echo world >>1.txt

这时候1.txt 内容如下:
hello
world
```

- ``<``重定向符

作用：从文件中而不是从键盘中读入命令输入。

语法：

```bat
set /p var1=<test.txt  @rem 将从stdin输入重定向为从文件输入
echo %var1%
Hello world

//文件test.txt中的内容为Hello world
```

### Part6-常用技巧

#### 1,获取管理员权限

```bat
PUSHD %~DP0 & cd /d "%~dp0"
%1 %2
mshta vbscript:createobject("shell.application").shellexecute("%~s0","goto :runas","","runas",1)(window.close)&goto :eof
:runas
  
::填写自己的脚本

```
