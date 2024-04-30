---
title: Trojan科学上网：搭建
date: 2020-03-08 13:40:47
categories:
- 软件
- 科学上网
tags:
- fq
---

## 目录

- [简介](#简介)
- [部署](#部署)
- [常见问题总结](#常见问题总结)
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
- Trojan-Go访问原理
  ```
  当一个客户端试图连接Trojan-Go的监听端口时，会发生下面的事情：
  如果TLS握手成功，检测到TLS的内容非Trojan协议（有可能是HTTP请求，或者来自GFW的主动探测）。Trojan-Go将TLS连接代理到本地127.0.0.1:80上的HTTP服务。这时在远端看来，Trojan-Go服务就是一个HTTPS网站。
  如果TLS握手成功，并且被确认是Trojan协议头部，并且其中的密码正确，那么服务器将解析来自客户端的请求并进行代理，否则和上一步的处理方法相同。
  如果TLS握手失败，说明对方使用的不是TLS协议进行连接。此时Trojan-Go将这个TCP连接代理到本地127.0.0.1:1234上运行的HTTPS服务（或者HTTP服务），返回一个展示400 Bad Reqeust的HTTP页面。fallback_port是一个可选选项，如果没有填写，Trojan-Go会直接终止连接。虽然是可选的，但是还是强烈建议填写。
  ```

## 部署

### `准备工作`

- ►更新系统安装环境
  ```bash
  # 更新系统
  yum update -y  #CentOS
  apt update -y  #Debian
  # 安装curl｜wget
  yum install -y curl  #CentOS
  yum install -y wget  #CentOS
  apt-get install wget  #Debian Ubuntu
  apt-get install curl  #Debian Ubuntu
  ```
- ►安装开心版宝塔(免手机验证)
  - 直接安装V7.7.0的版本，之后的版本都会验证userInfo.json，虽然网上有大把的开心版，但不敢用。
    ```bash
    # Centos/Ubuntu/Debian安装命令 独立运行环境（py3.7）
    curl -sSO https://raw.githubusercontent.com/8838/btpanel-v7.7.0/main/install/install_panel.sh && bash install_panel.sh
    # 备用安装链接，适用于不能访问GitHub的服务器。
    curl -sSO http://d.moe.ms/AAAAA/btpanel-v7.7.0/install/install_panel.sh && bash install_panel.sh
    ```
  - 屏蔽手机号
    ```bash
    sed -i "s|bind_user == 'True'|bind_user == 'XXXX'|" /www/server/panel/BTPanel/static/js/index.js
    ```
  - 删除强制绑定手机js文件
    ```bash
    rm -f /www/server/panel/data/bind.pl
    ```
  - 手动解锁宝塔所有付费插件为永不过期
    ```
    文件路径：/www/server/panel/data/plugin.json，搜索字符串："endtime": -1全部替换为"endtime": 999999999999
    给plugin.json文件上锁防止自动修复为免费版：chattr +i /www/server/panel/data/plugin.json
    ```
- ►开启 BBR 加速
  - 开启Debian10自带的BBR加速
    ```bash
    # 本脚本只针对 Debian≥9 或是 CentOS≥8 以上的系统，可以开启系统自带BBR加速。
    echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
    echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
    sysctl -p
    lsmod | grep bbr
    ```
  - 四合一 BBR Plus / 原版BBR / 魔改BBR
    ```bash
    # Centos 7, Debian 8/9, Ubuntu 16/18 测试通过, 不支持 OVZ
    wget -N --no-check-certificate "https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh" && chmod +x tcp.sh && ./tcp.sh
    ```

### `安装Trojan-go面板`

> 提前准备好trojan-web面板域名和trojan服务域名

1. Jrohy的一键Trojan面板脚本
  ```bash
  #安装/更新
  source <(curl -sL https://git.io/trojan-install)
  #卸载
  source <(curl -sL https://git.io/trojan-install) --remove
  ```
2. 修改trojan-web端口
  ```bash
  /etc/systemd/system/trojan-web.service
  在/usr/local/bin/trojan web 后面添加 -p port
  systemctl daemon-reload
  systemctl restart trojan-web
  ```
3. trojan设置
  ```bash
  # 修改端口
    /usr/local/etc/trojan/config.json
  # 申请证书报错
    netstat -ntlp
    采用命令 sudo fuser -k 80/tcp 强制 杀掉进程
    nginx
  # 连接被墙的外网就No route to host：
    切换trojan-go版本解决
  ```
4. 更改Trojan-Go配置文件(可选)
  ```
  # 找到VPS目录文件 /usr/local/etc/trojan/config.json ，备份一份（若是把类型切换回来可以恢复使用Trojan）
  # 需要增加WS等其他Trojan-Go所支持的模块，增加完成后保存并在面板重启Trojan-GO服务
  "websocket": {
      "enabled": true,
      "path": "/DFE4545DFDED/",
      "host": "你的域名"
  },
  "mux": {
      "enabled": true,
      "concurrency": 8,
      "idle_timeout": 60
  }
  ```

## 常见问题总结

- Failed to set locale, defaulting to C.UTF-8解决方法
  - https://blog.csdn.net/ba476/article/details/124448981
- Invalid version. The only valid version for X509Req is 0.
  - https://www.cnblogs.com/jscs/p/17484543.html

## 参考链接

<https://ybfl.net/sites/158.html>
<https://v2rayssr.com/bbr.html>
<https://v2rayssr.com/trojancdn.html>
<https://github.com/Jrohy/trojan>
<https://www.youtube.com/watch?v=tC5bER5iHyE>
<https://kejilion.blogspot.com/2023/10/vps.html>


## 结束语

- 未完待续...