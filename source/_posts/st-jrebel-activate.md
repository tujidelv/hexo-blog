---
title: IDEA热部署插件JRebel
date: 2020-01-13 16:26:33
categories:
- 后端
- Java
tags:
- jetbrains
---

## 目录

- [简介](#简介)
- [正篇](#正篇)
- [参考链接](#参考链接)
- [结束语](#结束语)

## 简介

JRebel是一款JVM插件，它使得代码片段/配置文件修改后不用重启项目立即生效。IDEA上原生是不支持热部署的，一般更新了Java文件后要手动重启Tomcat服务器才能生效，浪费时间浪费生命。

目前对于IDEA热部署最好的解决方案就是安装JRebel插件。

## 正篇

### 激活

1. 搭建反向代理服务器
    - 下载OS相对应的工具包：<https://github.com/ilanyu/ReverseProxy/releases>
    - 执行Shell命令：`nohup ./ReverseProxy_linux_amd64 -l "0.0.0.0:9999" &`
    - Demo JRebel address：【http://127.0.0.1:9999/{GUID}】, with any email.
2. 生成GUID 
    - 在线生成GUID地址：<https://www.guidgen.com>
    - 一键地址直接使用: 【https://jrebel.lvzhiqiang.top/f3b9f9e8-4084-415e-8bac-9c856985a0a4】

### 使用

1. 打开jrebel激活面板,选择Connect to online licensing service.
2. 如果失效刷新GUID替换就可以！

## 参考链接

- [撸了个反代工具, 可用于激活JRebel](http://blog.lanyus.com/archives/317.html)

## 结束语

- 未完待续...

