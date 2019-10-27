---
title: Linux 系统应用：添加开机启动服务/脚本
date: 2019-03-09 15:27:42
categories:
- 操作系统
tags:
- linux
---

# Linux 系统应用：添加开机启动服务/脚本

## 目录

- [简介](#简介)
- [正篇](#正篇)
- [参考链接](#参考链接)
- [结束语](#结束语)

## 简介

线上环境对服务/脚本的自启动设置.

## 正篇

- `添加开机自启服务`
    ```
    在CentOS 7中添加开机自启服务非常方便，只需要两条命令(以Jenkins为例)：
        systemctl enable/disable jenkins.service #设置jenkins服务为自启动服务
        systemctl start/restart/stop/status jenkins.service #启动jenkins服务
    兼容CentOS 6：
        service network start/restart/stop/status
        chkconfig network on/off
        chkconfig --list
    ```

- `添加开机自启脚本`
    ```
    在centos7中增加脚本有两种常用的方法，以脚本autostart.sh为例：
        #!/bin/bash
        #description:开机自启脚本
        /usr/local/tomcat/bin/startup.sh  #启动tomcat
     -------------------   
    方法一：
        1、赋予脚本可执行权限（/opt/script/autostart.sh是你的脚本路径）
            chmod +x /opt/script/autostart.sh
        2、打开/etc/rc.d/rc.local或/etc/rc.local文件，在末尾增加如下内容
            su - user -c '/opt/script/autostart.sh'
        3、在centos7中，/etc/rc.d/rc.local的权限被降低了，所以需要执行如下命令赋予其可执行权限
            chmod +x /etc/rc.d/rc.local
    方法二：
        1、将脚本移动到/etc/rc.d/init.d或/etc/init.d目录下
            mv /opt/script/autostart.sh /etc/rc.d/init.d
        2、增加脚本的可执行权限
            chmod +x  /etc/rc.d/init.d/autostart.sh
        3、添加脚本到开机自动启动项目中
            cd /etc/rc.d/init.d
            chkconfig --add autostart.sh
            chkconfig autostart.sh on
    ```

## 参考链接

<https://blog.csdn.net/wang123459/article/details/79063703?utm_source=copy>

## 结束语

- 未完待续...
