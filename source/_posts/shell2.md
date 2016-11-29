title: shell 简单教程(中)
date: 2016-11-26 21:13:37
tags: Linux
categories: Linux
---
这次我们介绍一下条件语句和循环语句
## **条件语句**
### **简单条件语句**
使用格式如下
```bash
if condition
then
command1 if condition is true or if exit status
of condition is 0 (zero)
...
...
fi
```
编辑showfile文件
```bash
#!/bin/sh
#
#Script to print file
#
if cat $1
then
echo -e "\n\nFile $1, found and successfully echoed"
fi
```
执行
```bash
$ chmod 755 showfile
$./showfile foo
```
编辑ispostive文件
```bash
#!/bin/sh
#
# Script to see whether argument is positive
#
if test $1 -gt 0
then
echo "$1 number is positive"
fi
```
执行
```bash
$ chmod 755 ispostive
$ ispostive 5
```
### **判断条件 **
test 或者 [expr] 作用于整数，文件类型，字符串

|数学符号	|含义	|一般表达式	       |shell中的test   |shell中的[expr]|
|:------|:---------------|:------------|:-----------------|:---------------|
|-eq   |等于	                |5 == 6	     |if test 5 -eq 6	|if [ 5 -eq 6 ]    |
|-ne   |不等于	            |5 != 6	     |if test 5 -nq  6	|if [ 5 -nq  6 ]   |
|-lt	   |小于	                |5 < 6	         |it test 5 -lt 6	    |if [ 5 -lt 6 ]       |
|-le	   |小于或等于	    |5 <= 6	    |if test 5 -le 6	    |if [ 6 -le 6 ]      |
|-gt	   |大于	                |5 > 6	        |it test 5 -gt 6     |if [5 -gt 6 ]       |
|-ge   |大于或等于	   |5 >= 6	   |if test 5 -ge 6	   |if [ 5 -ge 6]       |

#### **对字符串的比较**

|操作符	|含义      |
|:---------------------|:----------------------------------------|
|string1 = string2	 |string1 是否等于string2                    |
|string1 != string2 |	string1 是否不等于 string2             |
|string1	                     |string1 不为 NULL 或 不是没定义   |
|-n string1	             |string1不为NULL 并且存在              |
|-z string1 	            |string1 为NULL 并且存在                 |

#### **对于文件或者是目录**

|test	|含义|
|:---------|:-------------------------------------------------|
|-s file	|不为空文件                                                             |
|-f file    |文件是否存在或是普通文件并且不是目录   |
|-d dir	|目录是否存在且不是一个普通文件                |
|-w file	|文件是否可写                                                        |
|-r file	|文件是否可读                                                        |
|-x file	|文件是否可执行                                                   |


#### **逻辑运算**

|运算符	|含义|
|:-----------------------------------|:--------|
|! expression	                                |逻辑非  |
|expression1 -a expression2	|逻辑与  |
|expression1 -o expression2	|逻辑或  |

### **条件语句 if - else - fi **
使用格式如下
```bash
if condition
then
　　condition is zero (true - 0)
　　execute all commands up to else statement

else
　　if condition is not true then
　　execute all commands up to fi
fi
```
编辑isnump_n文件
```bash
#!/bin/sh
#
# Script to see whether argument is positive or negative
#
if [ $# -eq 0 ]
then
echo "$0 : You must give/supply one integers"
exit 1
fi

if test $1 -gt 0
then
echo "$1 number is positive"
else
echo "$1 number is negative"
fi
```
执行
```bash
$ chmod 755 isnump_n
$ isnump_n 5
$ isnump_n -45 
$ isnump_n
$ isnump_n 0
```
### **嵌套if - else - fi**
使用格式如下
```bash
if condition
	then
		if condition
		then
			.....
			..
			do this
		else
			....
			..
			do this
		fi
	else
		...
    .....
    do this
    fi
```
编辑nestedif.sh

```bash
osch=0

echo "1. Unix (Sun Os)"
echo "2. Linux (Red Hat)"
echo -n "Select your os choice [1 or 2]? "
read osch

if [ $osch -eq 1 ] ; then

     echo "You Pick up Unix (Sun Os)"

else #### nested if i.e. if within if ######
            
       if [ $osch -eq 2 ] ; then
             echo "You Pick up Linux (Red Hat)"
       else
             echo "What you don't like Unix/Linux OS."
       fi
fi
```
执行
```bash
$ chmod +x nestedif.sh
$ ./nestedif.sh
$ ./nestedif.sh
$ ./nestedif.sh
```
### **多层if - else - fi**
```bash
if condition
then
　　condition is zero (true - 0)
　　execute all commands up to elif statement
elif condition1 
then
　　condition1 is zero (true - 0)
　　execute all commands up to elif statement 
elif condition2
then
　　condition2 is zero (true - 0)
　　execute all commands up to elif statement 
else
　　None of the above condtion,condtion1,condtion2 are true (i.e. 
　　all of the above nonzero or false)
　　execute all commands up to fi
fi
```
编辑elf
```bash
#
#!/bin/sh
# Script to test if..elif...else
#
if [ $1 -gt 0 ]; then
  echo "$1 is positive"
elif [ $1 -lt 0 ]
then
  echo "$1 is negative"
elif [ $1 -eq 0 ]
then
  echo "$1 is zero"
else
  echo "Opps! $1 is not number, give number"
fi
```
执行
```bash
$ chmod 755 elf
$ ./elf 1
$ ./elf -2
$ ./elf 0
$ ./elf a
```
## **shell中的循环**
### **for 循环**
格式如下：
```bash
for { variable name } in { list }
do
　　execute one for each item in the list until the list is
　　not finished (And repeat all statement between do and done)
done
```
编辑testfor文件
```bash
for i in 1 2 3 4 5
do
echo "Welcome $i times"
done
```
执行
```bash
$ chmod +x testfor
$ ./testfor
```
编辑mtable文件
```bash
#!/bin/sh
#
#Script to test for loop
#
#
if [ $# -eq 0 ]
then
echo "Error - Number missing form command line argument"
echo "Syntax : $0 number"
echo "Use to print multiplication table for given number"
exit 1
fi
n=$1
for i in 1 2 3 4 5 6 7 8 9 10
do
echo "$n * $i = `expr $i \* $n`"
done
```
执行
```bash
$ chmod 755 mtable
$ ./mtable 7
$ ./mtable
```
另一种格式如下
```bash
for (( expr1; expr2; expr3 ))
do
　　.....
　　...
　　repeat all statements between do and 
　　done until expr2 is TRUE
Done
```
编辑for2文件
```bash
for ((  i = 0 ;  i <= 5;  i++  ))
do
  echo "Welcome $i times"
done
```
执行
```bash
$ chmod +x for2
$ ./for2
```
嵌套for循环
编辑nestedfor文件
```bash
for (( i = 1; i <= 5; i++ ))      ### Outer for loop ###
do

    for (( j = 1 ; j <= 5; j++ )) ### Inner for loop ###
    do
          echo -n "$i "
    done

  echo "" #### print the new line ###

done
```
执行
```
$ chmod +x nestedfor.sh
$ ./nestefor.sh
```
编辑chessboard文件
```bash
for (( i = 1; i <= 9; i++ )) ### Outer for loop ###
do
   for (( j = 1 ; j <= 9; j++ )) ### Inner for loop ###
   do
        tot=`expr $i + $j`
        tmp=`expr $tot % 2`
        if [ $tmp -eq 0 ]; then
            echo -e -n "\033[47m "
        else
            echo -e -n "\033[40m "
        fi
  done
 echo -e -n "\033[40m" #### set back background colour to black
 echo "" #### print the new line ###
done
```
执行
```bash
$ chmod +x chessboard $ ./chessboard
```
### **while循环**
使用格式如下
```bash
while [ condition ]
do
　　command1
　　command2
　　command3
　　..
　　....
done
```
编辑nt1文件
```bash
#!/bin/sh
#
#Script to test while statement
#
#
if [ $# -eq 0 ]
then
   echo "Error - Number missing form command line argument"
   echo "Syntax : $0 number"
   echo " Use to print multiplication table for given number"
exit 1
fi
n=$1
i=1
while [ $i -le 10 ]
do
  echo "$n * $i = `expr $i \* $n`"
  i=`expr $i + 1`
done
```
执行
```bash
$ chmod 755 nt1
$./nt1 7
```
## **case语句**
使用格式如下
```bash
case $variable-name in
pattern1) command
　　...
　　..
　　command;;
pattern2) command
　　...
　　..
　　command;;
patternN) command
　　...
　　..
　　command;;
*) command
　　...
　　..
　　command;;
esac
```
编辑car文件
```bash
#
# if no vehicle name is given
# i.e. -z $1 is defined and it is NULL
#
# if no command line arg
if [ -z $1 ]
then
  rental="*** Unknown vehicle ***"
elif [ -n $1 ]
then
# otherwise make first arg as rental
  rental=$1
fi

case $rental in
   "car") echo "For $rental Rs.20 per k/m";;
   "van") echo "For $rental Rs.10 per k/m";;
   "jeep") echo "For $rental Rs.5 per k/m";;
   "bicycle") echo "For $rental 20 paisa per k/m";;
   *) echo "Sorry, I can not gat a $rental for you";;
esac
```

执行
```bash
$ chmod +x car
$ car van
$ car car
$ car Maruti-800
```
## **如何de-bug shell脚本**
使用格式
```bash
sh options {shell 脚本名}
```
或者是
```bash
bash options {shell 脚本名}
```
options可以是

-v 当shell读脚本时把脚本内容打印出来

-x 脚本中的命令会展开打印出来

编辑dsh1文件
```bash
#
# Script to show debug of shell
#
tot=`expr $1 + $2`
echo $tot
```
执行
```bash
$ chmod 755 dsh1.sh
$ ./dsh1.sh 4 5
$ sh -x dsh1.sh 4 5
$ sh -v dsh1.sh 4 5
```
第一个输出结果
```bash
9
```
第二个输出结果

```bash
+ expr 4 + 5
+ tot=9
+ echo 9
9
```
第三个输出结果
```bash
#
#script to show debug of shell
#
tot=`expr $1 + $2`
echo $tot
9
```
