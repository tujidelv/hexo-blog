---
title: Linux 常用程序包的安装与配置
date: 2019-03-09 17:07:04
categories:
- 操作系统
tags:
- linux
---

# Linux 常用程序包的安装与配置

## 目录

- [简介](#简介)
- [正篇](#正篇)
- [参考链接](#参考链接)
- [结束语](#结束语)

## 简介

整理在 Linux 环境下常用软件包的安装和配置.

## 正篇

### `JDK的安装和配置`

1. 检查 CentOS 有没有安装其他版本的 JDK,例如 opentJDK等等
    ```
    # java -version
    # rpm -qa | grep "java"
    -----------------------
    注：如果有的话,可配合rpm命令或者yum命令先卸载掉,其中yum命令能自动解决依赖等问题.
        eg：# rpm -e --nodeps tzdata-java-2016j-1.el6.noarch java-1.6.0-openjdk-1.6.0.41-1.13.13.1.el6_8.x86_64 java-1.7.0-openjdk-1.7.0.131-2.6.9.0.el6_8.x86_64
    ```
2. 下载 JDK 程序包,rpm或者tar.gz格式的都可以
    ```
    方法1：
        1. 去jdk官网找到指定版本的jdk,并复制其链接地址
        2. wget -P /opt/setups/ https://download.oracle.com/otn-pub/java/jdk/8u201-b09/42970487e3af4f5aa5bca3f542482c60/jdk-8u201-linux-x64.tar.gz?AuthParam=1552124414_039c05a3b27cb7d09e715788bf78b6e0
        3. 重命名即可
    方法2：
        1. 去jdk官网找到指定版本的jdk,下载到本地
        2. 通过rz等上传工具将程序包上传到/opt/setups目录中去
    ```
3. 解压到指定目录
    ```
    # mkdir -pv /usr/program
    # tar -zxf jdk-8u201-linux-x64.tar.gz -C /usr/program
    ```
4. 配置环境变量,使用vim编辑/etc/profile.d/my.sh文件
    ```
    # JDK
    JAVA_HOME=/usr/program/jdk1.8.0_201
    JRE_HOME=$JAVA_HOME/jre
    PATH=$JAVA_HOME/bin:$PATH
    CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
    export JAVA_HOME
    export JRE_HOME
    export PATH
    export CLASSPATH
    ```
5. 重新读取配置文件,使之生效
    ```
    # source /etc/profile.d/my.sh
    ```
6. 测试是否安装成功
    ```
    # java -version
    ```
    
### `TOMCAT的安装和配置`

1. 下载 TOMCAT 程序包,rpm或者tar.gz格式的都可以
    ```
    方法1：
        1. 去jdk官网找到指定版本的jdk,并复制其链接地址
        2. wget -P /opt/setups/ http://apache.fayea.com/tomcat/tomcat-8/v8.5.43/bin/apache-tomcat-8.5.43.tar.gz
        3. 重命名即可
    方法2：
        1. 去tomcat官网找到指定版本的tomcat,下载到本地
        2. 通过rz等上传工具将程序包上传到/opt/setups目录中去
    ```
2. 解压到指定目录
    ```
    # mkdir -pv /usr/program
    # tar -zxf apache-tomcat-8.5.34.tar.gz -C /usr/program
    ```
3. 设置 iptables 规则

    - 一种是关闭iptables,另一种是在iptables中添加允许规则(Tomcat默认端口是8080)
    - CentOS 6系统
        ```
        # iptables -I INPUT -p tcp -m tcp --dport 8080 -j ACCEPT
        # service iptables save
        # service iptables restart
        ```
    - CentOS 7系统
        ```
        # firewall-cmd --zone=public --add-port=8080/tcp --permanent 
        # firewall-cmd --reload
        ```
4. 测试安装好后的 Tomcat
    - 启动 Tomcat
        ```
        # sh /usr/program/apache-tomcat-8.5.34/bin/startup.sh
        # tail -200f /usr/program/apache-tomcat-8.5.34/logs/catalina.out
        ```
    - 访问 `http://服务器 IP 地址:8080/`
    - 停止 Tomcat
        ```
        # sh /usr/program/apache-tomcat-8.5.34/bin/shutdown.sh
        ```
5. 优化(待更新)

### `RocketMQ的集群部署`

详见"消息中间件之 RocketMQ"章节

### `Redis的集群部署`

详见"分布式缓存框架之 Redis&Ehcache"章节


## 参考链接

<https://github.com/judasn/Linux-Tutorial>

## 结束语

- 未完待续...
