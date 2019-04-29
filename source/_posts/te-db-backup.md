---
title: Mysql 数据库自动备份脚本
date: 2019-03-09 15:23:53
categories:
- 经典示例
tags:
- linux
---

# Mysql 数据库自动备份脚本

## 目录

- [简介](#简介)
- [正篇](#正篇)
- [参考链接](#参考链接)
- [结束语](#结束语)

## 简介

线上环境对 mysql 数据库的备份.

## 正篇

1. 将如下内容保存为 sh 文件，并保存至相关目录，例如：mysql-backup.sh
    ```
    #!/bin/bash
    # Mysql 数据库自动备份脚本

    # 数据库认证
    host=localhost
    username=root
    password=123456
    db_name=test
    # 备份路径
    backup_dir=/data/backup/mysql
    # 日期格式
    date=$(date +"%Y%m%d")
    # Dump数据库到SQL文件
    mysqldump -h$host -u$username -p$password $db_name > $backup_dir/$db_name-$date.sql
    #写创建备份日志
    echo "create $backup_dir/$db_name-$date.sql" >> $backup_dir/log.txt
     
    # 备份清理
    # 删除5天之前的就备份文件
    find $backup_dir/* -mtime +5 -exec rm {} \;
    ```
2. 通过 `crontab -e` 命令打开定时任务配置文件，添加如下代码，即可实现每天定时备份 mysql 数据库
    ```
    # 每天的23点50分执行备份
    50 23 * * * /data/backup/mysql-backup.sh
    ```

## 参考链接

## 结束语

- 未完待续...