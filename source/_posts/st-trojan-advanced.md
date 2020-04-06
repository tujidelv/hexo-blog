---
title: Trojan科学上网：进阶
date: 2020-04-06 18:28:31
categories:
- 软件工具
tags:
- fq
---

## 目录

- [简介](#简介)
- [开始](#开始)
- [参考链接](#参考链接)
- [结束语](#结束语)

## 简介

记录trojan的一些进阶使用技巧。

## 开始

### `Trojan全平台客户端`

- 前言
    - 解决了MAC或Windows系统上使用Trojan更方便的问题，不需要连接Scoks代理、启动trojan进程等这些步骤了。
    - 新版本的Clash/ClashX支持SSR/Trojan/V2ray等协议，支持节点订阅，不得不说Trojan客户端的软件匹配速度是越来越快了。
- Clash下载地址
    - Windows平台项目开源地址：[点击访问](https://github.com/Fndroid/clash_for_windows_pkg)
    - MacOS平台项目开源地址：[点击访问](https://github.com/yichengchen/clashX)
    - 安卓平台项目开源地址：[点击访问](https://github.com/Kr328/ClashForAndroid)
- 标准Clash配置文件
    ```
    # Shadowsocks的标准写法
    ## 第一种配置
    - name: "你的 SS 节点 1"               # 软件显示的节点名字
      type: ss                                  # 代理类型
      server: 1.2.4.8                          # 服务器 IP
      port: 443                                 #  端口号
      cipher: chacha20-ietf-poly1305   # 加密方法
      password: "password"                # SS 密码
      # udp: true                                #默认不开启
      
    ## 第二种配置
    ### Shadowsocks + simple-obfs   配置范本
    - name: "你的 SS 节点 2"
      type: ss
      server: 1.2.4.8
      port: 443
      cipher: chacha20-ietf-poly1305
      password: "password"
      plugin: obfs
      plugin-opts:                 
        mode: tls # or http               #  大部分选择 HTTP
        # host: bing.com                  #  伪装
    ```
    ```
    # v2ray的标准写法
    ## VMess 的配置
    - name: "你的 V2RAY 节点 1" # 软件显示的节点名字
      type: vmess # 代理类型
      server: v2rayssr.com  # 服务器 IP
      port: 443 #  端口号
      uuid: a3482e88-686a-4a58-8126-99c9df64b7bf
      alterId: 64  #额外的 ID
      cipher: auto
      #上面几行为必选参数
      #下面几行为可选参数  根据你的配置情况来
      # udp: true    #默认不开启
      # tls: true      #TLS 开启
      # skip-cert-verify: true     #默认不开启
      # network: ws    # 网路类型 WS HTTP 等
      # ws-path: /path  # 路径
      # ws-headers:     #默认不开启
      #  Host: v2rayssr.com    # HOST
      
    ## v2ray+ws+tls(+nginx)的源码配置
    - name: "主机测试"
      type: vmess
      server: www.v2rayssr.com
      port: 443
      uuid: dfdf8e0-c95d-4c74-b5d5-4a330969c8cb
      alterId: 2
      cipher: auto
      tls: true
      network: ws
      ws-path: /f2a5dfd0/
      Host: www.v2rayssr.com
    ```
    ```
    # Trojan Clash/ClashX 配置文件写法
    ## Trojan 的配置
      - name: "Trojan 节点测试"
        type: trojan
        server: server
        port: 443
        password: yourpsk
        # udp: true
        # sni: example.com # aka server name
        # alpn:
        #   - h2
        #   - http/1.1
        # skip-cert-verify: true
        
    ## 示例
       - name: "Trojan 主机测试" # 软件显示的节点名字
         type: trojan
         server: hk.v2rayssr.com # 服务器域名
         port: 443
         password: trojanpasswords #Trojan 密码
    ```
- Clash配置文件范例
    - 下面是Clash配置的一个标准设置（.yaml 文件）
    - 配置文件托管在 GitHub：[点击访问](https://raw.githubusercontent.com/V2RaySSR/Tools/master/clash.yaml)（不需富强）
    - 配置文件托管在 GitHub：[点击访问](https://github.com/V2RaySSR/Tools/blob/master/clash.yaml)（需要富强）

### `Trojan面板`

待更新...

## 参考链接

<https://www.v2rayssr.com/clashxx.html>

## 结束语

- 未完待续...