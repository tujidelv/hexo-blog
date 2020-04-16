---
title: 度盘命令行下载利器BaiduPCS-Go
date: 2020-04-16 14:32:42
categories:
- 软件工具
tags:
- fq
---

## 目录

- [简介](#简介)
- [开始](#开始)
- [常见问题](常见问题)
- [参考链接](#参考链接)
- [结束语](#结束语)

## 简介

- **[BaiduPCS-Go](https://github.com/iikira/BaiduPCS-Go)**是一个开源跨平台的度盘命令行客户端，为操作度盘提供实用功能。
- 该工具很大程度上解决了在VPS上下载百度云文件速度过慢的问题。
- 如果对纯命令形式的操作不习惯，可以参考**[baidupcs-web](https://github.com/liuzhuoling2011/baidupcs-web)**使用web界面操作。
---
>1.多平台支持, 支持 Windows, MacOS, Linux, 移动设备等。
2.百度帐号多用户支持。
3.[通配符](https://baike.baidu.com/item/%E9%80%9A%E9%85%8D%E7%AC%A6)匹配网盘路径和Tab自动补齐命令和路径。
4.**下载**网盘内文件, 支持多个文件或目录下载, 支持断点续传和单文件并行下载。
5.**上传**本地文件, 支持上传大文件(>2GB), 支持多个文件或目录上传。
6.**离线下载**, 支持http/https/ftp/电驴/磁力链协议。

## 开始

>Go语言程序, 可直接在[发布页](https://github.com/iikira/BaiduPCS-Go/releases)下载使用.
可在这里下载最新commit对应的测试版: <https://ci.appveyor.com/project/iikira/baidupcs-go/build/artifacts>
如果程序运行时输出乱码, 请检查下终端的编码方式是否为UTF-8.
如果未带任何参数运行程序, 程序将会进入仿Linux shell系统用户界面的cli交互模式, 可直接运行相关命令.
cli交互模式下, 光标所在行的前缀应为 BaiduPCS-Go >, 如果登录了百度帐号则格式为 BaiduPCS-Go:<工作目录> <百度ID>$
程序会提供相关命令的使用说明.

### `安装`

- 类Unix用户(Linux,MacOS等)
    >程序应在终端(Terminal)运行.
- Windows用户
    >程序应在命令提示符(Command Prompt)或PowerShell中运行,在mintty(例如: GitBash)可能会有显示问题.
    新手建议直接双击程序运行,进入仿Linux shell的cli交互模式.
- MacOS用户
    >安卓, 建议使用[Termux](https://termux.com)或[NeoTerm](https://github.com/NeoTerm/NeoTerm)或终端模拟器, 以提供终端环境.
     示例: [Android运行本项目程序参考示例](https://github.com/iikira/BaiduPCS-Go/wiki/Android-%E8%BF%90%E8%A1%8C%E6%9C%AC%E9%A1%B9%E7%9B%AE%E7%A8%8B%E5%BA%8F%E5%8F%82%E8%80%83%E7%A4%BA%E4%BE%8B), 有兴趣的可以参考一下.
     苹果iOS,需要越狱,在Cydia搜索下载并安装MobileTerminal,或者其他提供终端环境的软件.
     示例: [iOS 运行本项目程序参考示例](https://github.com/iikira/BaiduPCS-Go/wiki/iOS-%E8%BF%90%E8%A1%8C%E6%9C%AC%E9%A1%B9%E7%9B%AE%E7%A8%8B%E5%BA%8F%E5%8F%82%E8%80%83%E7%A4%BA%E4%BE%8B), 有兴趣的可以参考一下.

### `使用`

- 登录度盘账号
    ```
    1.常规登录百度帐号
        支持在线验证绑定的手机号或邮箱
        BaiduPCS-Go login
    2.使用百度BDUSS来登录百度帐号
        关于获取百度BDUSS可使用相关插件获取，Chrome推荐cookies.txt
        BaiduPCS-Go login -bduss=<BDUSS>
        例子BaiduPCS-Go login -bduss=1234567
    ```
- 常用选项
    ```
    USAGE:
        BaiduPCS-Go [global options] command [command options] [arguments...]
    ```
    ```
    GLOBAL OPTIONS:
          --verbose             启用调试 [$BAIDUPCS_GO_VERBOSE]
          --help, -h            show help
          --version, -v         print the version
    COMMANDS:
        tool                    工具箱
        help, h, ?, ？           Shows a list of commands or help for one command
      其他:
        clear, cls              清空控制台
        env                     显示程序环境变量
        run                     执行系统命令
        sumfile, sf             获取本地文件的秒传信息
        update                  检测程序更新
      百度帐号:
        login [-bduss]          登录百度账号(支持常规登录以及百度BDUSS来登录)
        loglist                 列出帐号列表
        logout                  退出百度帐号
        su [uid]                切换百度帐号
        who                     获取当前帐号
      百度网盘:
        cd [-l]                 切换工作目录(支持切换目录后自动列出目录下的文件和目录及支持通配符)
        cp                      拷贝文件/目录
        createsuperfile, csf    手动分片上传—合并分片文件
        download, d             下载文件/目录
        export, ep              导出文件/目录
        fixmd5                  修复文件MD5
        locate, lt              获取下载直链
        ls, l, ll               列出目录(支持-asc,-desc,-time,-name,-size排序)
        match                   测试通配符
        meta                    获取文件/目录的元信息
        mkdir                   创建目录
        mv                      移动/重命名文件/目录
        offlinedl, clouddl, od  离线下载
        pwd                     输出工作目录
        quota                   获取网盘配额
        rapidupload, ru         手动秒传文件
        recycle [l]             回收站
        rm                      删除文件/目录(被删除的文件或目录可在网盘文件回收站找回,对删除时要验证有奇效)
        search, s               搜索文件(不支持查找目录,默认在当前工作目录搜索)
        share                   分享文件/目录
        tree, t                 列出目录的树形图
        upload, u               上传文件/目录
      配置:
        config                  显示和修改程序配置项
    ```
- 下载文件/目录
    ```
    BaiduPCS-Go d <网盘文件或目录的路径1> <文件或目录2> <文件或目录3> ...
    可选参数
        --test          测试下载, 此操作不会保存文件到本地
        --ow            overwrite, 覆盖已存在的文件
        --status        输出所有线程的工作状态
        --save          将下载的文件直接保存到当前工作目录
        --saveto value  将下载的文件直接保存到指定的目录
        -x              为文件加上执行权限, (windows系统无效)
        --mode value    下载模式, 可选值: pcs, stream, locate, locate_pan, share, 默认为 locate, 相关说明见上面的帮助 (default: "locate")
        -p value        指定下载线程数 (default: 0)
        -l value        指定同时进行下载文件的数量 (default: 0)
        --retry value   下载失败最大重试次数 (default: 3)
        --nocheck       下载文件完成后不校验文件
    注意点
        下载的文件默认保存到程序所在目录的download/目录,支持自定义指定默认保存目录,重名的文件会自动跳过!
        eg：BaiduPCS-Go config set -savedir D:/Downloads
    下载模式说明
        pcs: 通过百度网盘的 PCS API 下载
        stream: 通过百度网盘的 PCS API, 以流式文件的方式下载, 效果同 pcs
        locate: 默认的下载模式。从百度网盘 Android 客户端, 获取下载链接的方式来下载
        locate_pan: 从百度网盘 WEB 首页获取下载链接来下载, 该下载方式需配合第三方服务器, 机密文件切勿使用此下载方式
        share: 从网盘文件的分享列表获取文件的下载链接来下载
    ```
- 上传文件/目录
    ```
    BaiduPCS-Go u <本地文件/目录的路径1> <文件/目录2> <文件/目录3> ... <目标目录>
    注意点
        上传默认采用分片上传的方式, 上传的文件将会保存到<目标目录>.
        遇到同名文件将会自动覆盖!!
        当上传的文件名和网盘的目录名称相同时, 不会覆盖目录, 防止丢失数据.
    ```
- 离线下载
    ```
    添加离线下载任务
        BaiduPCS-Go offlinedl add -path=<离线下载文件保存的路径> 资源地址1 地址2 ...
        添加任务成功之后, 返回离线下载的任务ID.
    精确查询离线下载任务
        BaiduPCS-Go offlinedl query 任务ID1 任务ID2 ...
    查询离线下载任务列表
        BaiduPCS-Go offlinedl list
    取消离线下载任务
        BaiduPCS-Go offlinedl cancel 任务ID1 任务ID2 ...
    删除离线下载任务
        BaiduPCS-Go offlinedl delete 任务ID1 任务ID2 ...
    清空离线下载任务记录, 程序不会进行二次确认, 谨慎操作!!!
        BaiduPCS-Go offlinedl delete -all
    ```

### `配置相关`

- 显示程序环境变量
    ```
    BAIDUPCS_GO_VERBOSE="0"                                 是否启用调试
    BAIDUPCS_GO_CONFIG_DIR="/root/.config/BaiduPCS-Go"      配置文件路径
    ```
- 显示和修改程序配置项
    ```
    # 显示配置
    BaiduPCS-Go config
    # 设置配置
    BaiduPCS-Go config set
    ```
    ```
    #设置下载文件的储存目录，默认存在/root/Downloads
    ./BaiduPCS-Go config set -savedir /home
    #设置下载最大并发量为200，建议值50~500，数值越大速度越高，但太高可能会出问题
    ./BaiduPCS-Go config set -max_parallel 200
    # 组合设置
    BaiduPCS-Go config set -max_parallel 150 -savedir D:/Downloads
    ```

## 常见问题

1. 下载速度慢/时快时慢
    >尝试调高下载最大并发量和下载缓存, 以下数据为参考数据
BaiduPCS-Go config set -max_parallel 400 -cache_size 65536

2. 文件名有空格,中括号,或者是特殊字符不能识别
    >使用双引号扩起文件名，或者在空格, 小括号, 中括号, 特殊字符前加一个反斜杠"\"
另外, 建议在命名文件时, 不要使用这些字符==


## 参考链接

<https://github.com/iikira/BaiduPCS-Go>
<https://www.moerats.com/archives/738>
<https://github.com/liuzhuoling2011/baidupcs-web>

## 结束语

- 未完待续...