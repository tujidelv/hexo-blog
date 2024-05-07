---
title: VPS搭建无人值守推流服务器
date: 2024-05-07 15:04:57
categories:
   - 软件
   - 科学上网
tags:
   - fq
---

## 目录

- [VPS搭建无人值守推流服务器](#VPS搭建无人值守推流服务器)
- [参考链接](#参考链接)
- [结束语](#结束语)

## 搭建无人值守推流服务器

### 简介

Youtube、哔哩哔哩、抖音、Tik Tok如何实现7×24小时在线直播进阶版，实现多平台无人直播，VPS搭建无人值守7×24小时推流服务器，轻松赚收益。

### 安装

1. 安装依赖包
    ```bash
    # Ubuntu/Debian系统安装
    apt update -y && apt install vim screen -y
    # Centos系统安装
    yum update -y && yum install vim screen -y
    ```
2. 安装ffmpeg
    ```bash
    # Ubuntu/Ddian系统安装ffmpeg
    apt install ffmpeg
    # Centos系统安装ffmpeg
    yum install epel-release
    rpm -v --import http://li.nux.ro/download/nux/RPM-GPG-KEY-nux.ro
    rpm -Uvh http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-5.el7.nux.noarch.rpm
    yum install ffmpeg ffmpeg-devel
    # 检查是否安装成功
    ffmpeg -version
    ```
3. 上传推流脚本和上传直播视频
   - 推流脚本作者地址：【[点击进入](https://lala.im/4816.html)】
   - 下载推流脚本：【[点击进入](https://drive.google.com/file/d/1mLEK-og-T6pGLoqqTl6DoKOju73f7fp-/view?usp=drive_link)】
   - 把想要做直播的视频和推流脚本上传到root目录下
   - 注意：目前支持循环推流mp4格式的视频，视频文件的名字不能含有空格或其他特殊符号。
4. 执行运行命令（注意：这里执行的是脚本的文件名，如果文件名更改，后面的stream.sh要改为对应的文件名）
    ```
    # 1.新打开一个窗口（注意：这个点很关键）
    screen -S stream
    # 2.执行脚本，选择2，输入上一步的推流地址，尽量在finalshell中间的输入框输入直播网址+直播码，/号拼接；然后输入直播视频所在文件路径，这样就成功直播了。
    bash stream.sh
    ```
5. 远程分离后台运行
    ```
    # 1.新打开一个页面，查找ID
    screen -ls
    # 2.然后远程detach
    screen -d id
    ```
6. 停止推流
    ```
    # 关闭对应的窗口
    screen -X -S id quit
    # 重新打开对应窗口
    screen -r id
    # 强制停止推流
    pkill -f "ffmpeg"
    ```

## 参考链接

<https://naiyous.com/4511.html>

## 结束语

- 未完待续...