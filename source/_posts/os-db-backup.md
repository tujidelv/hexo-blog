---
title: Mysql 数据库自动备份脚本
date: 2019-03-09 15:23:53
categories:
- 操作系统
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

1. 创建备份 Shell 脚本，并保存至相关目录，例如：mysql-backup.sh
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
2. 添加可执行权限
    ```
    ~]# chmod u+x mysql-backup.sh
    添加可执行权限之后先执行一下，看看脚本有没有错误，能不能正常使用；
    ~]# ./mysql-backup.sh
    ```
3. 通过 `crontab -e` 命令添加计划任务，添加如下代码，即可实现每天定时备份 mysql 数据库
    ```
    # 每天的23点50分执行备份
    50 23 * * * /data/backup/mysql-backup.sh
    ```

## 参考链接

## 结束语

- 未完待续...
- 附带`Crontab`的常用格式
```
第 1 列分钟 0～59
第 2 列小时 0～23（0 表示子夜）
第 3 列日 1～31
第 4 列月 1～12
第 5 列星期 0～6（0 表示星期天）
第 6 列要运行的命令

30 21 * * * /usr/local/apache/bin/apachectl restart
上面的例子表示每晚的 21:30 重启 apache。

45 4 1,10,22 * * /usr/local/apache/bin/apachectl restart
上面的例子表示每月 1、10、22 日的 4 : 45 重启 apache。

10 1 * * 6,0 /usr/local/apache/bin/apachectl restart
上面的例子表示每周六、周日的 1 : 10 重启 apache。

0,30 18-23 * * * /usr/local/apache/bin/apachectl restart
上面的例子表示在每天 18 : 00 至 23 : 00 之间每隔 30 分钟重启 apache。

0 23 * * 6 /usr/local/apache/bin/apachectl restart
上面的例子表示每星期六的 11 : 00 pm 重启 apache。

0 */1 * * * /usr/local/apache/bin/apachectl restart
每一小时重启 apache

#20160912 修正，感谢 @张琼的指正，之前写错了，*/1 和 * 表示的同样的意思，对于 / 的用法，可以参考另一篇文章 Crontab 中的除号到底怎么用？

0 23-7/1 * * * /usr/local/apache/bin/apachectl restart
晚上 11 点到早上 7 点之间，每隔一小时重启 apache

0 11 4 * mon-wed /usr/local/apache/bin/apachectl restart
每月的 4 号与每周一到周三的 11 点重启 apache

0 4 1 jan * /usr/local/apache/bin/apachectl restart
一月一号的 4 点重启 apache
```