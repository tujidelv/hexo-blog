---
title: NodeJS 札记
date: 2018-12-01 16:28:22
categories:
- 前端
- 开发语言
tags:
- nodejs
---

# NodeJS 札记

## 目录

- [简介](#简介)
- [发展历史](#发展历史)
- [准备工作](#准备工作)
- [模块与包](#模块与包)
- [基本模块](#基本模块)
- [Web开发](#Web开发)
- [参考链接](#参考链接)
- [结束语](#结束语)

## 简介

- Node.js 是一个能够在**服务器端**运行 JavaScript 的开源、 跨平台 **JavaScript 运行环境**。
- Node.js 采用 Google 开发的 V8 引擎运行 js 代码，使用**事件驱动**、**非阻塞**和**异步 I/O** 模型等技术来提高性能，可优化应用程序的传输量和规模。
- 特点：
    - 非阻塞、异步的 I/O
    - 事件和回调函数
    - 单线程（主线程单线程，后台 I/O 线程池）
    - 跨平台
- 用途：
    - Web 服务 API，比如 REST
    - 实时多人游戏
    - 在后台编写单线程的服务器
    - 基于 Web 的应用
    - 多客户端的通信，如即时通信
    
## 发展历史

1. 2004 年，非科班出身的Ryan Dahl(瑞安·达尔)，在纽约的罗彻斯特大学数学系读博士。
2. 2006 年，也许是厌倦了读博的无聊，他产生了"世界那么大， 我想去看看"的念头，做出了退学的决定，然后一个人来到智利的 Valparaiso 小镇。
3. 从那起，Ryan Dahl 不知道是否因为生活的关系，他开始学习网站开发了，走上了码农的道路。
4. 那时候 Ruby on Rails(Ruby 的一个 web 框架)很火，他也不例外的学习了它，后来的 Node.js 也是借鉴了它才写出来的。
5. 从那时候开始，Ryan Dahl 的生活方式就是接项目，然后去客户的地方工作，在他眼中，拿工资和上班其实就是去那里旅行。
6. Ryan Dahl 经过两年的工作后，成为了高性能 Web 服务器的专家，从接开发应用到变成专门帮客户解决性能问题的专家。
7. 期间他开始写一些开源项目帮助客户解决 Web 服务器的高并发性能问题，他尝试了很多种语言，发现很多语言虽然同时提供了同步 IO 和异步 IO，但是开发人员一旦用了同步 IO，他们就再也懒得写异步 IO 了，所以，最终，Ryan瞄向了 JavaScript。
    - 对于高性能，异步 IO、事件驱动是基本原则。
    - JavaScript 是单线程执行，根本不能进行同步 IO 操作，所以，JavaScript 的这一“缺陷”导致了它只能使用异步 IO。
8. 选定了开发语言，还要有运行时引擎，他曾考虑过自己写一个，不过明智地放弃了，因为 V8 就是开源的 JavaScript引擎。让 Google 投资去优化 V8，他只负责改造一下拿来用，还不用付钱，这个买卖很划算。
9. 2009 年 2 月，他开始着手编写 web.js，目的是用 JavaScript 去编写一个高性能的 Web 服务器。
10. 同年，Ryan Dahl 正式推出了基于JavaScript语言和V8引擎的开源Web服务器项目，命名为Node.js，并在GitHub上发布最初版本。
    - 虽然名字很土，但是 Node.js 第一次把 JavaScript 带入到后端服务器开发，加上世界上已经有无数的 JavaScript 开发人员，所以 Node.js 一下子就火了起来。
    - 在 Node.js 上运行的 JavaScript 相比其他后端开发语言，最大的优势是借助 JavaScript 天生的事件驱动机制加 V8 高性能引擎，使编写高性能 Web 服务轻而易举。
11. 2010 年 1 月，Node.js 的包管理器 npm 诞生。
12. 2010 年 12 月，Joyent 公司(美国的一个服务器供应商)赞助 Node.js 的开发，Ryan Dahl 加入旗下，全职负责 Node.js。
13. 2011 年 7 月，Node.js 在微软的帮助下发布了 windows 版本。
14. 2011 年 11 月，Node.js 超越 Ruby on Rails，称为 GitHub 上关注度最高的项目。
15. 2012 年 1 月，Ryan Dahl 离开 Node.js 项目。
16. 2014 年 12 月，Node.js 虽然由社区推动，但幕后一直由 Joyent 公司资助，由于一群开发者对 Joyent 公司的策略不满，Fedor Indutny 从 Node.js 项目 fork 出了 io.js 项目，决定单独发展，但两者实际上是兼容的。
17. 2015 年 1 月，Joyent 公司感觉势头不对，Node.js感觉要完，于是联合 IBM、Intel、微软成立了 Node.js 基金会。
18. 2015 年 9 月，正所谓"分久必合，合久必分”，分家后没多久，Joyent 公司表示要和解，于是 io.js 项目又决定回归 Node.js，Node.js 4.0发布。
    - 奇数版为开发版本,偶数版为稳定版本。
19. 2016 年，Node.js 6.0 发布。
20. 2017 年，Node.js 8.0 发布。

## 准备工作

- 下载与安装
    - 在 Git Bash 中输入 `node -v` 打印版本号即成功。
- IDEA 中安装 NodeJS 插件，方便在 js 文件中右键调试代码
    - 也可以在 Terminal 中输入 `node js文件名` 运行js文件。
    - 可在 `settings->Lnaguages & Frameworks->Node.js and NPM`中勾选`Coding assistance for Node.js`，来打开 Node.js 的核心代码提示。

## 模块与包
   
### 模块
 
- ECMAScript 标准的缺陷
    - ES5 中没有原生支持模块化，我们只能通过 script 标签引入js文件来实现模块化(ES6 有了)。
    - 标准库较少。
    - 没有标准接口。
    - 缺乏管理系统。
- CommonJS 规范
    - CommonJS 规范的提出，主要是为了弥补当前 JavaScript 没有模块化标准的缺陷。
    - CommonJS 规范为 JS 指定了一个美好的愿景， 希望 JS 能够在任何地方运行。
    - 在 Node.js 中为了对模块管理，引入了 CommonJS 规范，**从而大大提高了代码的复用性和可维护性。**
    - CommonJS 规范对模块的定义十分简单：**模块定义**，**模块引用**，**模块标识**。
- 模块定义
    - **在 Node.js 中，一个 js 文件就是一个模块。**
    - 在 Node.js 中，默认情况下每一个 js 文件中的 js 代码都是独立运行在一个函数中。
        - 而不是全局作用域，所以一个模块的中的变量和函数在其他模块中无法访问。
        - 实际上模块中的代码都是包装在一个函数中执行的，并且在函数执行时，同时传递进了5个实参
        ```
        var load = function (exports, require, module, __filename, __dirname) {
            //js文件内容
            ...
            // 参数解释
            exports
                - 该对象用来将变量或函数暴露到外部
            require
                - 函数，用来引入外部的模块
            module
                - module 代表的是当前模块本身
                - exports 就是 module 的属性
                - 既可以使用 exports 导出，也可以使用 module.exports 导出
            __filename
                - 当前模块的完整路径
            __dirname
                - 当前模块所在文件夹的完整路径
        }
        ```
        ```
        导出变量和函数
            - 通过exports只能使用.的方式来向外暴露内部变量或函数
                exports.xxx = xxx
            - 通过module.exports既可以使用.的形式，也可以直接赋值
                module.exports.xxx = xxx
                module.exports = {xxx:xxx,xxx:xxx,...}
        ```
        ```javascript
        console.log(arguments.callee + ""); //这个属性保存的是当前执行的函数对象
        console.log(arguments.length); //5
        ```
    - 在 Node.js 中有一个全局对象 global，它的作用和网页中 window 类似。
        - 在全局中创建的变量都会作为global的属性保存
        - 在全局中创建的函数都会作为global的方法保存
        ```javascript
        var a = 10;
        b = 20;
        console.log(global.a); //undefined
        console.log(global.b); //20
        ```
- 模块引用
    - 在 Node.js 中，**通过 require()函数来引入外部的模块**，该函数会返回一个对象，这个对象代表的是引入的模块。
    - 格式：var 变量 = require("模块的标识");
- 模块标识
    - **模块的标识就是模块的名字(去掉.js后缀的文件名)或文件路径(绝对路径,相对路径)**，也就是传递给 require()方法的参数。
    - 对于**核心模块**（npm 中下载的模块或者 Node.js 引擎提供的模块），直接使用模块的名字对其进行引入。
        ```javascript
        var fs = require("fs");
        var express = require("express");
        ```
    - 对于**文件模块**(用户自己创建的模块)，使用文件的路径来对其进行引入，如果是相对路径必须以./或 ../开头。
        ```javascript
        var router = require("./router");
        ```

### 包

- CommonJS 的包规范允许我们将一组相关的模块组合到一起，形成一组完整的工具。
    - **将多个模块组合为一个完整的功能，就是一个包。**
    - **增强版的模块。**
- CommonJS 的包规范由包结构和包描述文件两个部分组成。 
    - 包结构：用于组织包中的各种文件
        ```
        包实际上就是一个压缩文件，解压以后还原为目录。符合规范的目录，应该包含如下文件：    
            – package.json 包的描述文件，必须的 
            – bin 二进制可执行文件，一般都是一些工具包中才有
            – lib js文件 
            – doc 文档 
            – test 测试代码
        ```
    - 包描述文件：描述包的相关信息，类似于简历，以供外部读取分析
        ```
        它是一个json格式的文件，在它里面保存了包各种相关的信息，里面不能写注释
            - name 包名
            - version 版本
            - dependencies 依赖
            - devDependencies 开发依赖
            - main 包的主文件
            - maintainers 主要贡献者
            - keywords 关键字,方便搜索该包
        ```
- NPM(Node Package Manager) ：Node.js 的包管理器
    - CommonJS 包规范是理论，NPM 是其中一种实践。 
    - 通过 NPM 可以对 Node.js 中的第三方模块进行上传发布、下载安装、搜索和依赖等操作。
    - **NPM 会在安装完 Node.js 以后，自动安装。**
    - NPM 常用命令
        ```
        npm      //帮助说明
        npm -v      //查看npm的版本
        npm version      //查看所有模块的版本
        npm config list      //查看npm配置信息
        npm s/search 包名      //搜索模块包,需联网	
        -------------------------------------
        npm init      //初始化项目（创建package.json）,安装包时根据 package.json 是否存在来识别当前目录是一个包,不然可能会安装到其他地方
        npm i/install      //安装当前项目所依赖的包
        npm i/install 包名      //在当前目录安装指定的包
        npm i/install 包名 --save      //在当前目录安装指定的包并在package.json中添加依赖
        npm i/install 包名 --save-dev      //在当前目录安装指定的包并在package.json中添加开发依赖
        npm i/install 包名 -g       //全局模式安装指定的包（一般都是一些工具）,可以供命令行使用
        npm i/install 包名 -registry=地址      //从镜像源安装
        npm i/install 文件路径      //从本地安装
        npm r/remove 包名      //删除一个模块包
        npm r/remove 包名 --save     //删除一个模块包,并在package.json中移除依赖关系
        npm r/remove 包名 --save-dev     //删除一个模块包,并在package.json中移除开发依赖关系
        npm r/remove 包名 -g      //删除一个全局模式安装的模块包
        npm update 包名      //更新指定的已安装模块包的版本
        npm update 包名 -g      //更新指定的全局安装模块包的版本
        -------------------------------------
        npm config get prefix      //查看默认的全局安装路径
        npm config get cache      //查看默认的全局安装缓存路径
        npm config set prefix "E:\Ebook\JavaSE\develop\nodejs\node_global"      //设置默认的全局安装路径,需重新修改环境变量
        npm config set cache "E:\Ebook\JavaSE\develop\nodejs\node_cache"      //设置默认的全局安装缓存路径
        npm ls/list -g --depth 0      //查看全局安装过的包
        ```
    - 通过 NPM 下载的包都放到 node_modules 文件夹中，直接通过包名引入即可(**全局模式安装的需要配置NODE_PATH环境变量**)。
        ```
        Node.js 在使用模块名字来引入模块时，它会首先在当前目录的 node_modules 中寻找是否含有该模块
            如果有则直接使用，如果没有则去上一级目录的 node_modules 中寻找;
            如果有则直接使用，如果没有则再去上一级目录寻找，直到找到为止;
            直到找到磁盘的根目录，如果依然没有，则报错.
        ```
- CNPM ：淘宝 NPM 镜像
    - 这是一个完整 npmjs.org 镜像，你可以用此代替官方版本(只读)，同步频率目前为 10分钟 一次以保证尽量与官方服务同步。
    ```
    使用说明
        - 你可以使用我们定制的 cnpm (gzip 压缩支持) 命令行工具代替默认的 npm:
            $ npm install -g cnpm --registry=https://registry.npm.taobao.org
        - 或者你直接通过添加 npm 参数 alias 一个新命令:
            $ alias cnpm="npm --registry=https://registry.npm.taobao.org \
                --cache=$HOME/.npm/.cache/cnpm \
                --disturl=https://npm.taobao.org/dist \
                --userconfig=$HOME/.cnpmrc"
            # Or alias it in .bashrc or .zshrc
            $ echo '\n#alias for cnpm\nalias cnpm="npm --registry=https://registry.npm.taobao.org \
                --cache=$HOME/.npm/.cache/cnpm \
                --disturl=https://npm.taobao.org/dist \
                --userconfig=$HOME/.cnpmrc"' >> ~/.zshrc && source ~/.zshrc
    ```
    
## 基本模块

## Web开发

## 参考链接

- [Node.js - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/001434501245426ad4b91f2b880464ba876a8e3043fc8ef000)
- [淘宝 NPM 镜像](https://npm.taobao.org/)

## 结束语

- 未完待续...