---
title: Trojan科学上网：基础
date: 2020-03-08 13:40:47
categories:
- 软件工具
tags:
- fq
---

# Trojan科学上网：基础

## 目录

- [简介](#简介)
- [参考链接](#参考链接)
- [结束语](#结束语)

## 简介

- 介绍
    ```
    Trojan模仿了互联网上最常见的Https协议,以诱骗GFW封锁认为它就是Https,从而不被识别.
    Trojan处理来自外界的Https请求,如果是合法的,那么为该请求提供服务,否则将该流量转交给Web服务器Nginx,由Nginx为其提供服务.
    基于这个工作过程可以知道,Trojan的一切表现与Nginx一致,不会引入额外特征,从页达到无法识别的效果.
    ```
- Trojan与V2ray ws协议的区别
    ```
    V2ray是Nginx侦听443,数据->Nginx->V2ray;Trojan是自己侦听443,都是伪装成网站.
    ```
- 系统要求及脚本介绍
    ```
    1、系统支持centos7+/debian9+/ubuntu16+
    2、域名需要解析到VPS并生效。
    3、脚本自动续签https证书
    4、自动配置伪装网站，位于/usr/share/nginx/html/目录下，可在脚本中自行替换其中内容
    5、请不要在任何生产环境使用一键脚本，此条适用于本站所有脚本，专门用来科学上网的VPS可以随意使用。
    6、trojan不能用CDN，不要开启CDN
    7、如果你在用谷歌云、阿里云等产品的时候，需要在控制台开放80、443端口。
    9、可参考ssr文章安装相关的BBR加速
    ```

## 部署

### `服务端`

1. 下载并执行脚本
    ```
     [root@host ssr]# wget --no-check-certificate https://raw.githubusercontent.com/atrandys/trojan/master/trojan_mult.sh
     [root@host ssr]# chmod +x trojan_mult.sh
     [root@host ssr]# ./trojan_mult.sh 2>&1 | tee trojan_mult.log
    ```
![抱歉,图片休息了](st-trojan-basic/st-trojan-basic-001.png "trojan脚本主界面")

2. 选择安装trojan，然后输入解析到VPS的域名并回车（不要带http://），开始安装，然后等待安装完成即可。
    - **注：脚本中有关于Nginx的相关文件/目录需替换成自己服务器对应的文件/目录,否则可能启动Trojan失败。**
![抱歉,图片休息了](st-trojan-basic/st-trojan-basic-002.png "trojan安装完成")
3. 安装完成后，会展示一条下载地址，复制地址，并下载下来即可。

### `客户端`



## 参考链接

## 结束语