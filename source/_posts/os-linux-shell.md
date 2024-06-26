---
title: Linux 系统入门：Shell 脚本
date: 2019-01-02 20:51:18
categories:
- 运维
- Linux
tags:
- linux
---

# Linux 系统入门：Shell 脚本

## 目录

- [简介](#简介)
- [正篇](#正篇)
- [参考链接](#参考链接)
- [结束语](#结束语)

## 简介

shell脚本编程笔记...

## 正篇

### 前言

- `shebang`
    ```
    #!/bin/bash
    #!/usr/bin/python
    #!/usr/bin/perl
    ```
- `运行脚本`
    ```
    1.给予执行权限,通过具体的文件路径指定文件执行	
        eg: ~]# ./first.sh
    2.直接运行解释器,将脚本作为解释器程序的参数运行
    ```
- `bash自定义退出状态码`
    ```
    exit [n]：
    	注：脚本中一旦遇到exit命令,脚本会立即终止;终止退出状态取决于exit命令后面的数字
    注：如果未给脚本指定退出状态码，整个脚本的退出状态码取决于脚本中执行的最后一条命令的状态码
    ```
- `bash脚本编程之用户交互`
    ```
    # read [option]... [name ...]    从标准输入读入一行,并可将其切割成字段对位放入变量中 
        -p 'PROMPT'：添加提示信息
            也可这样写：$ echo -n "Enter a user name:";read name
            对bash而言,在同一行中使用分号隔开,2个命令会顺序执行
        -t TIMEOUT：设置命令等待的超时时间,单位为秒,在交互中用户如果超时没输入可以在脚本中给默认值
    # bash -n /path/to/some_script
    	检测脚本中的语法错误,不能检查逻辑错误
    # bash -x /path/to/some_script
        调试(单步)执行,它能显示其中执行的每一个代码的详细过程
    ```
    ```
    #!/bin/bash
    # Version: 0.0.1
    # Author: MageEdu
    # Description: read testing

    read -p "Enter a disk special file: " diskfile
    [ -z "$diskfile" ] && echo "Fool" && exit 1

    if fdisk -l | grep "^Disk $diskfile" &> /dev/null; then
        fdisk -l $diskfile
    else
        echo "Wrong disk special file."
        exit 2
    fi
    ```

### 变量

强类型：定义变量时必须指定类型，参与运算必须符合类型要求;调用未声明的变量会产生错误
弱类型：无需指定类型,默认均为字符型,参与运算会自动进行隐式转换;变量无须事先定义可直接调用

---
- `变量类型会决定数据存储格式,存储空间大小,参与运算种类`
    ```
    字符型：
    数值型：整型,浮点型
        declare -i name：声明一个整型变量
    ```
- `变量命名法则`
    ```
    1.不能使用程序中的保留字,如if,for等
    2.只能使用数字,字母及下划线,且不能以数字开头
    3.见名知义
    ```
- `只读变量`
    ```
    readonly name
    declare -r name
    ```
- `bash中变量种类`
    ```
    根据变量的生效范围等标准：
    	本地变量：生效范围为当前shell进程,对当前shell之外的其它shell进程,当前shell的子进程均无效	
    		变量赋值：name="value",将赋值符号后面的数据存储于变量名指向的内存空间
    			value：可以使用引用
    				1.可以是直接字串：name="username"	
    				2.变量引用：name="$username"
    				3.命令引用：name=`COMMAND`,name=$(COMMAND),命令执行的返回值	
    		变量引用：${name},$name
    			单引号：强引用，其中的变量引用不会被替换为变量值,而保持原字符串
    			双引号：弱引用，其中的变量引用会被替换为变量值
    		显示已定义的所有变量：
    			set
    		销毁变量：
    			unset name
    	环境变量：生效范围为当前shell进程及其子进程
    		变量声明\赋值：
    			export name=VALUE
    			declare -x name=VALUE
    		变量引用：同本地变量
    		显示所有环境变量：
    			export
    			env
    			printenv
    		销毁变量：同本地变量
    	局部变量：生效范围为当前shell进程中某代码片段(通常指函数)
    	位置变量：用于让脚本在脚本代码中调用通过命令行传递给它的参数
    		$1,$2,...：对应调用第1,第2等参数

    		$0：命令本身

    		$*：传递个脚本的所有参数(当做一个字符串)	
    		$@：传递个脚本的所有参数(多个字符串)
    		$#：传递个脚本的参数的个数
    	特殊变量：$?
    ```
    ![抱歉,图片休息了](os-linux-shell/os-linux-shell-001.png)

### 运算符

- `逻辑运算符`
    ```
    短路与：&&
    短路或：||
    逻辑异或：^
    逻辑非：!
    ```
- `算数运算符`
    ```
    +,-,*,/,%,++,--,
    ----------
    实现算数运算：
    	1.let var=算数表达式
    	2.var=$[算数表达式]
    	3.var=$((算数表达式))	
    	4.var=$(expr arg1 arg2 arg3 ...)
    		乘法符号有些场景中需要转义
		
    	bash中有内建的随机数生成器：$RANDOM
    	    eg: ~]# echo $[$RANDOM%60+1]
    ```
    ![抱歉,图片休息了](os-linux-shell/os-linux-shell-002.png)
- `赋值运算符`
    ```
    增强型赋值：+=,-=,*=,/=,%=,
    	let varOPERvalue
    		eg:let count+=1
    	自增：let var+=1 <==> let var++
    	自减：let var-=1 <==> let var--
    ```

### 条件测试

判断某需求是否满足,需要由测试机制来实现,专用的测试表达式需要由测试命令辅助完成测试过程.

---
- `测试命令`
    ```
    test EXPRESSION
    [ EXPRESSION ]
    [[ EXPRESSION ]]

    注：EXPRESSION前后必须有空白字符,否则语法错误
    ```
- `bash的测试类型`
    ```
    数值测试：
    	-gt：是否大于
    	-ge：是否大于等于
    	-eq：是否等于
    	-ne：是否不等于
    	-lt：是否小于
    	-le：是否小于等于
    字符串测试：
    	==：是否等于
    		也可以用=					
    	>：是否大于
    	<：是否小于
    	!=：是否不等于
    	=~：左侧字符串是否能够被右侧的PATTERN所匹配
    		此表达式一般用于[[  ]]中
    	-z "STRING"：测试字符串是否为空,空则为真,不空则为假
    	-n "STRING"：测试字符串是否为不空,不空则为真,空则为假
	
    	注：用于字符串比较时用到的操作数都应该使用引号
    文件测试：
    	存在性测试：
    		-a FILE
    		-e FILE：文件存在性测试，存在为真，否则为假
    	存在性及类别测试：
    		-b FILE：是否存在且为块设备文件
    		-c FILE：是否存在且为字符设备文件
    		-d FILE：是否存在且为目录文件
    		-f FILE：是否存在且为普通文件
    		-h/L FILE：是否存在且为符号链接文件
    		-p FILE：是否存在且为命名管道文件
    		-S FILE：是否存在且为套接字文件
    	文件权限测试：
    		-r FILE：是否存在且可读
    		-w FILE：是否存在且可写
    		-x FILE：是否存在且可执行
    	文件特殊权限测试：
    		-g FILE：是否存在且拥有sgid权限
    		-u FILE：是否存在且拥有suid权限
    		-k FILE：是否存在且拥有sticky权限
    	文件大小测试：
    		-s FILE：是否存在且非空
    	文件是否打开测试：
    		-t fd：fd表示文件描述符是否已经打开且与某终端相关	

    		-N FILE：文件自上一次被读取之后是否被修改过
    		-O FILE：当前有效用户是否为文件属主
    		-G FILE：当前有效用户是否为文件属组
    	双目测试：
    		FILE1 -ef FILE2：FILE1与FILE2是否指向同一个设备上的相同inode
    		FILE1 -nt FILE2：FILE1是否新于FILE2
    		    常用于备份中
    		FILE1 -ot FILE2：FILE1是否旧于FILE2
    		    常用于备份中
    ```
    ![抱歉,图片休息了](os-linux-shell/os-linux-shell-003.png)
- `组合测试条件`
    ```
    逻辑运算：
    	第一种方式：
    		COMMAND1 && COMMAND2
    		COMMAND1 || COMMAND2
    		! COMMAND
    	第二种方式：必须使用测试命令进行
    		EXPRESSION1 -a EXPRESSION2
    		EXPRESSION1 -o EXPRESSION2
    		! EXPRESSION
    ```
    ![抱歉,图片休息了](os-linux-shell/os-linux-shell-004.png)
    
### 流程控制语句

- `顺序执行`
- `选择执行`
    ```
    判断条件：
        a. 根据bash命令的执行状态结果(true或false),具体取决于用到的命令 
        b. 测试命令
    if语句可嵌套使用
    ```
    ```
    单分支：
        if 判断条件; then
            条件为真的分支代码
        fi
    双分支：
        if 判断条件; then
            条件为真的分支代码
        else
            条件为假的分支代码
        fi
    多分支：自上而下进行条件判断，第一次遇到为“真”的条件时，执行其分支，而后结束；
        if CONDITION1; then
            if-true
        elif CONDITION2; then
            if-ture 
        elif CONDITION3; then
            if-ture 
        ...
        esle
            all-false
        fi
    ```
- `循环执行`
    ```
    循环：
        循环次数已知：遍历
            for
        循环次数未知：
            while
            until
    循环可以嵌套
    ```
    ```
    for循环：
        for 变量名 in 列表; do
            循环体(要执行的代码；可能要执行n遍)
        done
        ---------------------
        执行机制：
            依次将列表中的元素赋值给“变量名”; 每次赋值后都要执行一次循环体; 直到列表中的元素耗尽，循环结束; 
        列表生成方式：
            (1) 直接给出列表；
            (2) 整数列表；
                (a) {start..end}
                (b) $(seq [start [step]] end) 
            (3) 返回列表的命令；例如ls /var等等
                $(COMMAND)
            (4) 文件名通配机制；
                例如for file in /var/*;do
            (b) 变量引用；
                $@, $*
    while循环：
        while 测试条件; do
            循环体
        done
        ---------------------
        执行机制：
            测试条件为真，进入循环；测试条件为假，退出循环；
            测试条件一般通过变量来描述，需要在循环体不变量地改变变量的值，以确保某一时刻测试条件为假，进而结束循环；
    until循环：
        until 测试条件; do
            循环体
        done
        ---------------------
        执行机制：
            测试条件为假，进入循环；测试条件为真，退出循环；
            测试条件一般通过变量来描述，需要在循环体不变量地改变变量的值，以确保某一时刻测试条件为真，进而结束循环；
    ```

### 练习

- 写一个脚本,实现如下功能:如果user1用户存在,就显示其存在,否则添加之;显示添加的用户的id号等信息
    ```
    #!/bin/bash
    id user1 &> /dev/null && echo "user1 exists." || useradd user1
    id user1
    ```
- 写一个脚本,完成如下功能:如果root用户登录了当前系统,就显示root用户在线,否则说明其未登录
    ```
    #!/bin/bash
    w | grep '^root\>' &> /dev/null && echo "root logged." || echo "root not logged."
    ```
- 写一个脚本,计算/etc/passwd文件中的第10个用户和第20个用户的ID之和
    ```
    #!/bin/bash
    userid1=$(head -n 10 /etc/passwd | tail -n 1 | cut -d: -f3)
    userid2=$(head -n 20 /etc/passwd | tail -n 1 | cut -d: -f3)
    useridsum=$[$userid1+$userid2]
    echo "uid sum: $useridsum"
    ```
- 写一个脚本,传递两个文件路径作为参数给脚本,计算这两个文件中所有空白行之和
    ```
    #!/bin/bash
    spaceline1=$(grep '^[[:space:]]*$' $1 | wc -l)
    spaceline2=$(grep '^[[:space:]]*$' $2 | wc -l)
    echo "The sum of space line: $[$spaceline1+$spaceline2]"
    ```
- 写一个脚本,统计/etc,/var,/usr目录中共有多少个一级子目录和文件
    ```
    #!/bin/bash
    sumEtc=$(ls -l /etc | grep '^[dbclsp]' | wc -l)
    sumVar=$(ls -l /var | grep '^[dbclsp]' | wc -l)
    sumUsr=$(ls -l /usr | grep '^[dbclsp]' | wc -l)
    echo "The total count of files: $[${sumEtc}+${sumVar}+${sumUsr}] lines."
    ```
- 写一个脚本,接受一个文件路径作为参数,如果参数个数小于1,则提示用户"至少应该给一个参数",并立即退出;如果参数个数不小于1,则显示第一个参数所指向的文件中的空白行数
    ```
    #!/bin/bash
    [ "$#" -lt 1 ] && echo "The must one arg." && exit 20
    [ -e "$1" ] || { echo "No such file or directory" && exit 30;}
    echo "The blankspace is $(grep '^[[:space:]]*$' $1 | wc -l) lines."
    ```
---
- 写一个脚本,添加用户
    ```bash
    #!/bin/bash
    #
    
    if [ $# -lt 1 ]; then
            echo "At least one argument."
            exit 1
    fi
    
    if id $1 &> /dev/null; then
            echo "$1 exists."
            exit 0
    else
            useradd $1
            [ $? -eq 0 ] && echo "$1" | passwd --stdin $1 &> /dev/null && exit 0 || exit 2
    fi
    ```
- 用户键入文件路径，脚本来判断文件类型
    ```bash
    #!/bin/bash
    #

    read -p "Enter a file path: " filename

    if [ -z "$filename" ]; then
        echo "Usage: Enter a file path."
        exit 2
    fi

    if [ ! -e $filename ]; then
        echo "No such file."
        exit 3
    fi

    if [ -f $filename ]; then
        echo "A common file."
    elif [ -d $filename ]; then
        echo "A directory."
    elif [ -L $filename ]; then
        echo "A symbolic file."
    else
        echo "Other type."
    fi
    ```
---
- 添加10个用户, user1-user10；密码同用户名
    ```bash
    #!/bin/bash
    #

    if [ ! $UID -eq 0 ]; then
        echo "Only root."
        exit 1
    fi

    for i in {1..10}; do
        if id user$i &> /dev/null; then
            echo "user$i exists."
        else
            useradd user$i
            if [ $? -eq 0 ]; then
                echo "user$i" | passwd --stdin user$i &> /dev/null
                echo "Add user$i finished."
            fi
        fi
    done
    ```
- 判断某路径下所有文件的类型
    ```bash
    #!/bin/bash
    #

    for file in $(ls /var); do
        if [ -f /var/$file ]; then
        echo "Common file."
        elif [ -L /var/$file ]; then
        echo "Symbolic file."
        elif [ -d /var/$file ]; then
        echo "Directory."
        else
        echo "Other type."
        fi
    done
    ```

## 参考链接

## 结束语

- 未完待续...