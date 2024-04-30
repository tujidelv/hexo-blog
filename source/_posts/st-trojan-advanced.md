---
title: Trojan科学上网：客户端
date: 2020-04-06 18:28:31
categories:
- 软件
- 科学上网
tags:
- fq
---

## 目录

- [简介](#简介)
- [Trojan全平台客户端](#Trojan全平台客户端)
- [参考链接](#参考链接)
- [结束语](#结束语)

## 简介

记录trojan的一些进阶使用技巧。

## Trojan全平台客户端

### `Windows`

- v2rayN
  1. Github下载[v2rayN](https://github.com/2dust/v2rayN/releases)；网盘下载<img src="st-trojan-advanced/dowload.png" width="16" height="16" align="center" />[网盘下载](https://pan.baidu.com/s/1s5gDIPgvJN2TdHnp-YsC7A?pwd=8888) `提取码8888`。
  2. 点击左上角服务器图标添加节点。支持剪贴板、二维码、添加[Trojan]等方式。
  3. 选中节点按回车键或者右键选择设为活动服务器，然后主界面下方-》系统代理-》选择自动配置系统代理，软件图标变成红色即表示已经开启翻墙。
  ```
  1. 不能翻墙怎么解决？
    1. 修改v2rayN的内核。设置-》参数设置-》Core类型设置，可以尝试对节点切换不同的内核。
    2. 同步系统时间。
    3. 打开浏览器无痕模式窗口。
    4. 多切换节点。
    5. 下载旧版本的v2rayN软件。
  2. 能翻墙但是网速很慢怎么解决？
    1. 设置-》路由设置-》绕过大陆，双击添加规则，在Domain中添加以下域名，一行一个域名，然后右击上移置顶。
    2. `fonts.googleapis.com,translate.googleapis.com,gstatic.com`。
  ```
- Trojan-Qt5
  1. Github下载[Trojan-Qt5](https://github.com/Trojan-Qt5/Trojan-Qt5/releases)；网盘下载<img src="st-trojan-advanced/dowload.png" width="16" height="16" align="center" />[网盘下载](https://pan.baidu.com/s/1_CYwT6VDaZPBSHMrEvs3iQ?pwd=8888) `提取码8888`。
  2. 菜单栏连接-》添加。支持剪贴板、二维码、手动添加[Trojan]、`config.json`等方式。
  3. 选中节点点击连接，开启翻墙。
  4. 右键图标可更改系统代理模式，包括直连模式、PAC模式、全局模式。
- Qv2ray
  1. Github下载[Qv2ray](https://github.com/Qv2ray/Qv2ray/releases)、[v2ray-core](https://github.com/v2ray/v2ray-core/releases)；网盘下载<img src="st-trojan-advanced/dowload.png" width="16" height="16" align="center" />[网盘下载](https://pan.baidu.com/s/1N5ZRDOYkZaH7oWg9cxziBQ?pwd=8888) `提取码8888`。
  2. 同Mac版。
---
- 浏览器插件
  同MAC版。

### `Mac`

- Qv2ray
  1. Github下载[Qv2ray](https://github.com/Qv2ray/Qv2ray/releases)、[v2ray-core](https://github.com/v2ray/v2ray-core/releases)、[trojan-go](https://github.com/p4gefau1t/trojan-go/releases)；网盘下载<img src="st-trojan-advanced/dowload.png" width="16" height="16" align="center" />[网盘下载](https://pan.baidu.com/s/1N5ZRDOYkZaH7oWg9cxziBQ?pwd=8888) `提取码8888`。
  2. 文件内容主要包括 Qv2ray客户端、Qv2ray v2ray核心、Qv2ray插件、Qv2ray Trojan-go插件核心（可选）。
  3. 设置中文语言，点击首选项-》常规设置，语言选中 zh_CN，保存后重启即可切换为简体中文。
  4. 设置v2ray核心，将下载好的v2ray核心解压后放入`/Applications/qv2ray.app/Contents/`，点击首选项-》内核设置，将v2ray可执行文件路径设置为解压好的核心文件夹下的V2ray可执行文件即可，资源目录设置成解压好的核心文件夹即可，然后点击一次"检查v2ray核心设置"，确认核心无误后会弹窗v2ray版本信息。
  5. 安装协议插件，点击插件-》打开本地插件目录，将下载好的插件(后缀.so)拖入至弹出的文件夹内，重启Qv2ray再点击"插件"菜单即可看到已安装的插件。
  6. 添加节点后，可选中节点右键选择连接到此服务器开启翻墙。
  ```
  1. (可选)设置Trojan-Go插件核心，在插件列表中找到Trojan-Go插件，并点击设定，点击选择并定位到解压好的核心文件路径下选择trojan-go可执行文件即可，点击"Test Run"弹窗版本信息即设置成功。
  2. TODO
  ```
- Trojan-Qt5
  1. Github下载[Trojan-Qt5](https://github.com/Trojan-Qt5/Trojan-Qt5/releases)；网盘下载<img src="st-trojan-advanced/dowload.png" width="16" height="16" align="center" />[网盘下载](https://pan.baidu.com/s/1_CYwT6VDaZPBSHMrEvs3iQ?pwd=8888) `提取码8888`。
  2. 同Windows版。
---
- Shadowrocket
  1. 打开 AppStore，登录美区 Apple ID 账号，美区 Apple ID 注册教程<https://youtu.be/hyopGvMVN1A>。
  2. 搜索小火箭 Shadowrocket，需要购买后才能下载使用。注意不支持Intel芯片的电脑，支持M1-M3芯片的电脑。
  3. 点击全局路由设置翻墙模式。默认是配置，指的是分流规则模式，就是国内网站走直连，国外网站走代理，这个模式会更节省流量；代理，指的是全局模式，就是所有的网站都走代理。
- Quantumult X
  1. 打开 AppStore，登录美区 Apple ID 账号，美区 Apple ID 注册教程同上。
  2. 搜索圈X Quantumult X，需要购买后才能下载使用。
  3. 右击右上角的小风车图标设置翻墙模式。3个图标分别是规则分流模式、全部直连、全部代理。
  4. Quantumult X 新手入门教程：<https://github.com/kjfx/QuantumultX>
---
- 浏览器插件
  1. 去<https://github.com/trojan-gfw/trojan/releases>下载对应平台trojan官方客户端
     ![抱歉,图片休息了](st-trojan-advanced/st-trojan-basic-003.png "trojan客户端")
  2. 安装SwitchyOmega插件，<img src="st-trojan-basic/dowload.png" width="16" height="16" align="center" />[网盘下载](https://pan.baidu.com/s/1nN60uMMsit4XNYS90eASaw) `提取码8cp7`；并设置情景模式。
     ![抱歉,图片休息了](st-trojan-advanced/st-trojan-basic-004.png "SwitchyOmega设置")

### `Android`

- v2rayNG
  1. Github下载[v2rayNG](https://github.com/2dust/v2rayNG/releases)；网盘下载<img src="st-trojan-advanced/dowload.png" width="16" height="16" align="center" />[网盘下载](https://pan.baidu.com/s/1R960U-r8URwrH9jMgrT_oQ?pwd=8888) `提取码8888`。
  2. 点击右上角+号添加节点。支持剪贴板、二维码、手动输入[Trojan]等方式。
  3. 点击右下角开关图标开启翻墙，然后点击下方灰色部分查看能不能使用及延迟大小；重新点击该图标即关闭翻墙。
  4. 点击左上角图标进入设置选项，找到路由设置=》预定义规则，默认是全局代理，一般选择绕过局域网及大陆地址而后代理，这个会更节省流量。
- Igniter
  1. Github下载[igniter](https://github.com/trojan-gfw/igniter/releases)；网盘下载<img src="st-trojan-advanced/dowload.png" width="16" height="16" align="center" />[网盘下载](https://pan.baidu.com/s/1LKOjYQWgtGexbo6IO00WgA?pwd=8888) `提取码8888`。
  2. 分别输入域名、端口、密码，可开启过滤大陆域名/IP，点击连接开启翻墙。
  ![抱歉,图片休息了](st-trojan-advanced/st-trojan-basic-005.png "Igniter客户端")

### `IOS`

- Shadowrocket
  同MAC版。

## 参考链接

<https://naiyous.com/3717.html>
<https://v2rayssr.com/trojancdn.html>
<https://github.com/Loyalsoldier/v2ray-rules-dat>
<https://johnshall.github.io/Shadowrocket-ADBlock-Rules-Forever>
[SwitchyOmega规则GFWList](https://github.com/gfwlist/gfwlist)

## 结束语

- 未完待续...