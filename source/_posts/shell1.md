title: Shell 简单教程(上)
date: 2016-11-24 21:11:05
tags: Linux
categories: Linux
---
经常使用Linux的用户对shell肯定不陌生，由于经常使用shell脚本，因此记录了一些学shell的笔记，也算是总结吧。
### **第一个shell脚本**
使用vim编辑第一个脚本
```bash
$ vim first.sh
```
将以下内容输入到shell脚本中
```bash
clear
echo "Hello $USER"
echo "Today is \c ";date
echo "Number of user login : \c" ; who | wc -l
echo "Calendar"
cal
exit 0
```
执行时需要更改以下权限
```bash
$ chomd 755 first.sh
$ ./first.sh
```
或者直接执行
```bash
$ sh first.sh
```
### **系统变量**
写完第一个脚本，现在开始熟悉以下系统变量。

表中列出了一些系统变量

|系统变量名称	                                          |变量名的意义           |
|:-------------------------------------------|:----------------------| 
|BASH=/bin/bash	shell                            |名                                  |
|BASH_VERSION=1.14.7(1)	                  |shell版本名               |
|COLUMNS=80	                                          |屏幕中的第几列       |
|HOME=/home/vivek	                              |home目录                 |
|LINES=25	                                                  |屏幕中的第几列       |
|LOGNAME=students	                              |当前的登录名           |
|OSTYPE=Linux	                                          |操作系统名                |
|PATH=/usr/bin:/sbin:/bin:/usr/sbin  |	PATH的设置             |
|PS1=[\u@\h \W]\$	                                   |提示设置                   |
|PWD=/home/students/Common       |	当前工作目录      |
|SHELL=/bin/bash	                                   |shell名                      |
|USERNAME=vivek	                                   |当前登录的用户名|


### **变量的定义**
定义的格式如下：

```
variable name=value
```

例如
```bash
$ no=10
$ echo $no
```
注意变量名是区分大小写的

no 与 No 不是一个变量

定义一个NULL变量， 可以如下

```bash
$ vech=
$ vech=""
```
### **echo命令**
介绍一下echo命令的选项

```bash
echo [options] [string, variables...]
```

其中options可以为


|options | 含义                                                       |
|:--------|:--------------------------------------|
|-n          |不作为一行输出，即输出后不换行|
|-e          |将下面的反斜杠解释成转义字符：|
|\\a        |警报                                                        |
|\\b        |删除                                                        |
|\\c        |不换行                                                    |
|\\n        |新建一行                                               |
|\\r         |回车键                                                    |
|\\t         |水平tab                                                 |
|\\\\       |反斜杠                                                   |

例如

```bash
$ echo -e "An apple a day keeps away \a\t\tdoctor\n"
```

### **打印彩色字符串**
```bash
$ echo -e "\033[34m   Hello Colorful  World!"
```
1) 第一个\033是换码符

2) 用[34m设置屏幕的前背景是蓝色

|character or letter	 | use in CSI   |  例子|
|:----|:----------------------------------------------------------|:-----------------------------------------|
 |h	 |设置ANSI模式	                                                                   |echo -e "\\033[h"                                 |
|l 	     |清除ANSI模式	                                                                  | echo -e "\\033[l"                                   |
|m 	 |设置输出字的颜色以及是否粗体                                 |	 echo -e  "\\033[35m Hello World"|
|q 	 |设置num lock，caps lock，scroll lock开还是关|	 echo -e "\\033[2q"                               |
|s 	| 存储当前光标的x,y坐标还有属性                               |	 echo -e "\\033[7s"                                |
|u 	| 重新存储当前光标的x,y坐标还有属性	                      | echo -e "\\033[8u"                                |
 

其中m参数的意义如下

|参数	|意义　	|例子|
|:----|:----------------------------------------------------------|:-----------------------------------------|
|0	|设置默认的颜色模式	|
|1	|设置粗体强度	|\$ echo -e "I am \033[1m BOLD \\033[0m Person"|
|2	|设置暗淡强度	|\$ echo -e "\033[1m  BOLD \\033[2m DIM  \\033[0m"|
|5	|设置闪烁强度	|\$ echo -e "\\033[5m Flash!  \\033[0m"|
|7	|设置相反的效果	|\$ echo -e "\\033[7m Linux OS! Best OS!! \\033[0m"|
|11	|显示特殊控制字符图形字符。如按下Ａlt键和数字键盘按178，放开两个键;没有将被打印。试一下示例中的输出 | \$ press alt + 178   \$  echo -e "\\033[11m" \$ press alt + 178   \$ echo -e "\\033[0m"$ press alt + 178 |
|25	|删除闪烁效果|	 |
|27	|删除相反效果|	 |
|30 - 37	|设置前景颜色：31 - 红色 32 - 绿色  xx - 自己试一下| \$ echo -e "\\033[31m I am in Red"|
|40 - 47	|设置背景颜色xx - 自己试一下|  \$ echo -e "\\033[44m Wow!!!"|


q参数的意义如下

|参数	|意义|
|:------|:-----------------------------------------|
|0	         |关掉所有键盘的LED灯                         |
|1	         |Scroll lock LED 打开，其他的关掉 |
|2     	|Num lock LED 打开，其他的关掉   |
|3        |Caps lock LED 打开，其他的关掉   |

### **算数运算**
expr op1 math-operator op2
示例
```bash
$ expr 1 + 3
$ expr 2 - 1
$ expr 10 / 2
$ expr 20 % 3
$ expr 10 \* 3
$ echo `expr 6 + 3`
```

注意乘法使用 \* 而不是 *
注意最后一个示例不是单引号，是ESC键下面的键的符号
用双引号或单引号只是打印字符串, 如下
```bash
$ echo "expr 6 + 3" # It will print expr 6 + 3
$ echo 'expr 6 + 3' # It will print expr 6 + 3
```

双引号： 在双引号里面的内容会将特殊字符替换
单引号： 在单引号里面的内容不会变，直接打印
反引号(即ESC键下面的键)：里面的内容作为命令执行之后替换

如下：
```bash
$ echo "Today is date"
$ echo 'Today is date'
$ echo "Today is `date`"
$ echo 'Today is `date`'
```
### **命令退出状态**
1) 如果返回值是0, 则说明命令执行成功
2) 如果返回值不是0, 则说明命令没有执行成功，或者是脚本中一些命令没有执行成功
使用 \$?
用echo \$?查看上一条命令的执行状态
```bash
$ ls
$ echo $?
```
### **关于变量**
读取变量
读取变量名的格式如下：
```bash
read variable1, variable2,...variableN
```
编辑sayH文件
```bash
echo "Your first name please:"
read fname
echo "Hello $fname, Lets be friend!"
```
下面执行
```bash
$ chmod 755 sayH
$ ./sayH
```
如果一行使用多个命令，命令之间使用；隔开
```bash
$ date;who
```
如果一个脚本有参数，在脚本内 \$0 是脚本名，\$1 是第一个参数， \$2 是第二个参数，\$# 是传递的参数个数， \$@ 或 $* 是所有的参数

编辑demo文件
```bash
#!/bin/sh
#
# Script that demos, command line args
#
echo "Total number of command line argument are $#"
echo "$0 is script name"
echo "$1 is first argument"
echo "$2 is second argument"
echo "All of them are :- $* or $@"
```
执行
```bash
$ chmod 755 demo
$ ./demo Hello World
```
### **管道**
使用格式如下
```bash
command1 | command2
```
```bash
$ who | wc -l
```
第一条命令的输出结果作为第二条命令的输入
 bc是计算命令
```bash
$bc
5 + 2 
5 > 12
5 == 10
5 != 2
5 == 5
12 < 2
```


|表达式     |含义|答案|bc的结果|
|:--------|:-----------------|:---------|:---------|
 |5> 12	|5是否大于12	  |NO          |0             |
|5 == 10	|5是否等于10	  |NO          |0             |
|5 != 2	|5是否不等于2 |YES          |1             |
|5 == 5	|5是否等于5	  |YES	        |1             |
|12 < 2	|12是否小雨2	  |NO	        |0             |
