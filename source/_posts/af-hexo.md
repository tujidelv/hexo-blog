---
title: Hexo 札记
date: 2018-12-01 18:25:23
categories:
- 应用框架
tags:
- hexo
- nodejs
---

# Hexo 札记

## 目录

- [简介](#简介)
- [准备工作](#准备工作)
- [快速入门](#开始搭建)
- [进阶使用](#进阶使用)
- [参考链接](#参考链接)
- [结束语](#结束语)

## 简介

- 中文官网
    - <http://hexo.io/zh-cn/>
- 作者 Tommy Chen
    - <https://zespia.tw/>
- 个人理解
    - Hexo 是一个基于 Node.js 快速、简洁且高效的博客框架，可以将 Markdown 文件快速的生成静态网页，托管在 GitHub Pages 上。
- 官网释义
    > Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。
- 特点
    ```
    1.超快速度
        Node.js 所带来的超快生成速度，让上百个页面在几秒内瞬间完成渲染。
    2.支持 Markdown
        Hexo 支持 GitHub Flavored Markdown 的所有功能，甚至可以整合 Octopress 的大多数插件。
    3.一键部署
        只需一条指令即可部署到 GitHub Pages, Heroku 或其他网站。
    4.丰富的插件
        Hexo 拥有强大的插件系统，安装插件可以让 Hexo 支持 Jade, CoffeeScript。
    ```

## 准备工作

- Git 安装
    ```
    Tujide.lv@tujide MINGW64 ~
    $ git --version
    git version 2.15.1.windows.2
    ```
- Node.js 安装
    ```
    Tujide.lv@tujide MINGW64 ~
    $ npm -v
    6.4.1
    ```
- 设置 Node.js 源为淘宝 NPM 镜像
    - Node.js 官方源默认是：<https://r.cnpmjs.org/>，但是由于在国外，说不定你使用的时候就抽风无法下载任何软件，所以我们决定暂时使用淘宝提供的源，淘宝源官网：<http://npm.taobao.org/>。
    - 安装时间不一定很快，耐心等待一会。
    ```
    Tujide.lv@tujide MINGW64 ~
    $ npm install -g cnpm --registry=https://registry.npm.taobao.org
    npm WARN deprecated socks@1.1.10: If using 2.x branch, please upgrade to at least 2.1.6 to avoid a serious bug with socket data flow and an import issue introduced in 2.1.0
    E:\Ebook\JavaSE\develop\nodejs\node_global\cnpm -> E:\Ebook\JavaSE\develop\nodejs\node_global\node_modules\cnpm\bin\cnpm
    +cnpm@6.0.0
    added 633 packages from 844 contributors in 120.981s
    
    Tujide.lv@tujide MINGW64 ~
    $ npm list -g --depth 0
    E:\Ebook\JavaSE\develop\nodejs\node_global
    `-- cnpm@6.0.0
    ```
    
## 开始搭建

- 安装 Hexo 框架
    - 因为国内的网络问题，有时候安装异常慢花了大半个小时都没效果，那就 `Ctrl + C` 停掉这次命令，执行 `npm uninstall -g hexo-cli`，然后重新再执行一次。
    - 安装时间不一定很快，耐心等待一会。
    ```
    Tujide.lv@tujide MINGW64 ~
    $ cnpm install -g hexo-cli
    Downloading hexo-cli to E:\Ebook\JavaSE\develop\nodejs\node_global\node_modules\hexo-cli_tmp
    Copying E:\Ebook\JavaSE\develop\nodejs\node_global\node_modules\hexo-cli_tmp\_hexo-cli@1.1.0@hexo-cli to E:\Ebook\JavaSE\develop\nodejs\node_global\node_modules\hexo-cli
    Installing hexo-cli's dependencies to E:\Ebook\JavaSE\develop\nodejs\node_global\node_modules\hexo-cli/node_modules
    [1/11] abbrev@^1.0.7 installed at node_modules\_abbrev@1.1.1@abbrev
    [2/11] object-assign@^4.1.0 installed at node_modules\_object-assign@4.1.1@object-assign
    [3/11] command-exists@^1.2.0 installed at node_modules\_command-exists@1.2.8@command-exists
    [4/11] minimist@^1.2.0 installed at node_modules\_minimist@1.2.0@minimist
    [5/11] tildify@^1.2.0 installed at node_modules\_tildify@1.2.0@tildify
    [6/11] chalk@^1.1.3 installed at node_modules\_chalk@1.1.3@chalk
    [7/11] bluebird@^3.4.0 installed at node_modules\_bluebird@3.5.3@bluebird
    [8/11] resolve@^1.5.0 installed at node_modules\_resolve@1.8.1@resolve
    [9/11] hexo-util@^0.6.0 installed at node_modules\_hexo-util@0.6.3@hexo-util
    [10/11] hexo-log@^0.2.0 installed at node_modules\_hexo-log@0.2.0@hexo-log
    fsevents@1.2.4 download from binary mirror: {"module_name":"fse","module_path":"./lib/binding/{configuration}/{node_abi}-{platform}-{arch}/","remote_path":"./v{version}/","package_name":"{module_name}-v{version}-{node_abi}-{platform}-{arch}.tar.gz","host":"https://cdn.npm.taobao.org/dist/fsevents"}
    platform unsupported hexo-fs@0.2.3 › chokidar@1.7.0 › fsevents@^1.0.0 Package require os(darwin) not compatible with your platform(win32)
    [fsevents@^1.0.0] optional install error: Package require os(darwin) not compatible with your platform(win32)
    [11/11] hexo-fs@^0.2.0 installed at node_modules\_hexo-fs@0.2.3@hexo-fs
    Recently updated (since 2018-11-18): 2 packages (detail see file E:\Ebook\JavaSE\develop\nodejs\node_global\node_modules\hexo-cli\node_modules\.recently_updates.txt)
      2018-11-22
        → hexo-util@0.6.3 › cross-spawn@4.0.2 › lru-cache@4.1.4 › yallist@^3.0.2(3.0.3) (07:22:36)
      2018-11-21
        → hexo-util@0.6.3 › cross-spawn@4.0.2 › lru-cache@^4.0.1(4.1.4) (08:14:09)
    All packages installed (175 packages installed from npm registry, used 7s(network 6s), speed 498.55kB/s, json 151(218.72kB), tarball 2.93MB)
    [hexo-cli@1.1.0] link E:\Ebook\JavaSE\develop\nodejs\node_global\hexo@ -> E:\Ebook\JavaSE\develop\nodejs\node_global\node_modules\hexo-cli\bin\hexo
    ```
- 创建 Hexo 项目
    ![抱歉,图片休息了](af-hexo/af-hexo-001.png "hexo 项目目录结构")
    - 现在初始化后不需要 `npm install` 了，它会默认执行该命令把相关依赖包下载到 node_modules 目录中，同时会自动生成 package-lock.json 文件用以纪录当前实际安装的各个插件包的具体来源和版本号。
    - 安装时间不一定很快，耐心等待一会。
    ```
    Tujide.lv@tujide MINGW64 /e/Ebook/JavaSE/workspace_idea
    $ hexo init hexo-blog
    INFO  Cloning hexo-starter to E:\Ebook\JavaSE\workspace_idea\hexo-blog
    Cloning into 'E:\Ebook\JavaSE\workspace_idea\hexo-blog'...
    remote: Enumerating objects: 68, done.
    remote: Total 68 (delta 0), reused 0 (delta 0), pack-reused 68
    Unpacking objects: 100% (68/68), done.
    Submodule 'themes/landscape' (https://github.com/hexojs/hexo-theme-landscape.git) registered for path 'themes/landscape'
    Cloning into 'E:/Ebook/JavaSE/workspace_idea/hexo-blog/themes/landscape'...
    remote: Enumerating objects: 21, done.
    remote: Counting objects: 100% (21/21), done.
    remote: Compressing objects: 100% (21/21), done.
    remote: Total 867 (delta 8), reused 0 (delta 0), pack-reused 846
    Receiving objects: 100% (867/867), 2.55 MiB | 1.31 MiB/s, done.
    Resolving deltas: 100% (457/457), done.
    Submodule path 'themes/landscape': checked out '73a23c51f8487cfcd7c6deec96ccc7543960d350'
    INFO  Install dependencies
    npm WARN deprecated titlecase@1.1.2: no longer maintained
    npm WARN deprecated postinstall-build@5.0.3: postinstall-build's behavior is now built into npm! You should migrate off of postinstall-build and use the new `prepare` lifecycle script with npm 5.0.0 or greater.
    
    > nunjucks@3.1.4 postinstall E:\Ebook\JavaSE\workspace_idea\hexo-blog\node_modules\nunjucks
    > node postinstall-build.js src
    
    npm notice created a lockfile as package-lock.json. You should commit this file.
    npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.4 (node_modules\fsevents):
    npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.4: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"})
    
    added 423 packages from 501 contributors and audited 4700 packages in 109.987s
    found 0 vulnerabilities
    
    INFO  Start blogging with Hexo!
    ```
- 启动 hexo 本地服务
    - 本地用浏览器访问：<http://localhost:4000/>，查看默认效果。
    - 如果要停止 hexo 服务：在 Git Bash 下按 `Ctrl + C` 即可。
    ```
    Tujide.lv@tujide MINGW64 /e/Ebook/JavaSE/workspace_idea/hexo-blog
    $ hexo server
    INFO  Start processing
    INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
    ```
- 更换其他主题
    - 由于默认主题太大众了，可以去这里找主题
        - hexo-theme：<https://hexo.io/themes/>
        - hexo-github-theme-list：<https://github.com/hexojs/hexo/wiki/Themes>
        - 有那些好看的 hexo 主题？：<http://www.zhihu.com/question/24422335>
    - 这里列出比较中意的 3 款(以 3-hexo 为例)
        - <https://github.com/theme-next/hexo-theme-next>
        - <https://github.com/MOxFIVE/hexo-theme-yelee>
        - <https://github.com/wizardforcel/hexo-theme-cyanstyle>
        - <https://github.com/yelog/hexo-theme-3-hexo>
    - 下载主题到 themes 文件夹
        - 如果以后你不自己修改这个主题的话，可以考虑 `git pull` 经常更新下作者的更新内容。
        - 下载时间不一定很快，耐心等待一会。
        ```
        Tujide.lv@tujide MINGW64 /e/Ebook/JavaSE/workspace_idea/hexo-blog
        $ git clone https://github.com/tujidelv/hexo-theme-3-hexo.git themes/3-hexo
        Cloning into 'themes/3-hexo'...
        remote: Enumerating objects: 12, done.
        remote: Counting objects: 100% (12/12), done.
        remote: Compressing objects: 100% (9/9), done.
        remote: Total 1854 (delta 3), reused 9 (delta 3), pack-reused 1842
        Receiving objects: 100% (1854/1854), 1.10 MiB | 937.00 KiB/s, done.
        Resolving deltas: 100% (1045/1045), done.
        
        Tujide.lv@tujide MINGW64 /e/Ebook/JavaSE/workspace_idea/hexo-blog/themes/3-hexo (master)
        $ git pull origin master
        From https://github.com/tujidelv/hexo-theme-3-hexo
         * branch            master     -> FETCH_HEAD
        Already up to date.
        ```
    - 修改 hexo 站点配置文件 _config.yml 使主题生效
    
        ![抱歉,图片休息了](af-hexo/af-hexo-002.png "hexo 主题修改")
    - 重新渲染主题静态内容并本地预览
        ```
        Tujide.lv@tujide MINGW64 /e/Ebook/JavaSE/workspace_idea/hexo-blog
        $ hexo clean && hexo g && hexo s
        INFO  Deleted database.
        INFO  Deleted public folder.
        INFO  Start processing
        INFO  Files loaded in 311 ms
        INFO  Generated: img/brown-papersq.png
        INFO  Generated: img/avatar.jpg
        INFO  Generated: img/weixin.jpg
        INFO  Generated: img/school-book.png
        INFO  Generated: index.html
        INFO  Generated: archives/index.html
        INFO  Generated: css/fonts/icomoon.eot
        INFO  Generated: img/alipay.jpg
        INFO  Generated: js/search.js
        INFO  Generated: css/hl_theme/atom-dark.css
        INFO  Generated: css/mobile.css
        INFO  Generated: css/hl_theme/atom-light.css
        INFO  Generated: css/hl_theme/brown-paper.css
        INFO  Generated: css/hl_theme/darcula.css
        INFO  Generated: css/hl_theme/github-gist.css
        INFO  Generated: css/hl_theme/github.css
        INFO  Generated: css/hl_theme/gruvbox-light.css
        INFO  Generated: css/hl_theme/gruvbox-dark.css
        INFO  Generated: css/hl_theme/kimbie-dark.css
        INFO  Generated: css/hl_theme/kimbie-light.css
        INFO  Generated: css/hl_theme/railscasts.css
        INFO  Generated: css/hl_theme/rainbow.css
        INFO  Generated: archives/2018/index.html
        INFO  Generated: css/hl_theme/school-book.css
        INFO  Generated: css/hl_theme/sublime.css
        INFO  Generated: css/hl_theme/sunburst.css
        INFO  Generated: css/hl_theme/zenbum.css
        INFO  Generated: css/fonts/icomoon.ttf
        INFO  Generated: archives/2018/11/index.html
        INFO  Generated: js/script.js
        INFO  Generated: css/fonts/icomoon.woff
        INFO  Generated: css/fonts/icomoon.svg
        INFO  Generated: css/style.css
        INFO  Generated: css/fonts/selection.json
        INFO  Generated: css/gitalk.css
        INFO  Generated: js/jquery.autocomplete.min.js
        INFO  Generated: 2018/11/28/hello-world/index.html
        INFO  Generated: js/gitalk.js
        INFO  38 files generated in 775 ms
        INFO  Start processing
        INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
        ```
- 创建 Github pages 并 SSH 授权
    - 创建好这个特殊仓库之后，在本地生成 SSH 秘钥并添加到 GitHub上，方便电脑上的 git 将内容到 Github 上。
    
    ![抱歉,图片休息了](af-hexo/af-hexo-003.png "hexo 仓库")
- 把本地的博客内容同步到 Github 上
    - 安装与 hexo 相关的 git 部署插件
        ```
        Tujide.lv@tujide MINGW64 /e/Ebook/JavaSE/workspace_idea/hexo-blog
        $ cnpm install hexo-deployer-git --save
        platform unsupported hexo-deployer-git@0.3.1 › hexo-fs@0.2.3 › chokidar@1.7.0 › fsevents@^1.0.0 Package require os(darwin) not compatible with your platform(win32)
        [fsevents@^1.0.0] optional install error: Package require os(darwin) not compatible with your platform(win32)
        √ Installed 1 packages
        √ Run 0 scripts
        deprecate hexo-deployer-git@0.3.1 › swig@^1.4.2 This package is no longer maintained
        Recently updated (since 2018-11-21): 1 packages (detail see file E:\Ebook\JavaSE\workspace_idea\hexo-blog\node_modules\.recently_updates.txt)
        √ All packages installed (183 packages installed from npm registry, used 18s(network 18s), speed 17.06kB/s, json 157(305.82kB), tarball 0B)
        ```
    - 修改 hexo 全局配置文件 _config.yml
        ```
        # Hexo Configuration
        ## Docs: https://hexo.io/docs/configuration.html
        ## Source: https://github.com/hexojs/hexo/
        
        # Site，网站，这一块区域主要是设置博客的主要说明，需要注意的是：每个冒号后面都是有一个空格，然后再书写自己的内容
        title: Tujidelv Code # 网站标题
        subtitle: 好记性不如烂笔头 # 网站副标题
        description: Tujidelv的技术小窝 # 网站描述，主要用于SEO，告诉搜索引擎一个关于您站点的简单描述，通常建议在其中包含您网站的关键词
        keywords:
        author: Tujide.lv # 您的名字，用于主题显示文章的作者
        language: zh-CN # 网站使用的语言，也可以用zh-Hans
        timezone:
        
        # URL，网址，这一块区域一般可以设置的是 url 这个参数，比如我要设置绑定域名的，这里就需要填写我的域名信息
        ## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
        ## 如果您的网站存放在子目录中，例如 http://yoursite.com/blog，则请将您的 url 设为 http://yoursite.com/blog 并把 root 设为 /blog/。
        url: https://www.lvzhiqiang.top # 网址
        root: / # 网站根目录
        permalink: :year/:month/:day/:title/ # 文章的永久链接格式
        permalink_defaults:
        
        # Directory，目录，这一块一般不需要修改
        source_dir: source # 资源文件夹，这个文件夹用来存放内容。
        public_dir: public # 公共文件夹，这个文件夹用于存放生成的站点文件。
        tag_dir: tags # 标签文件夹
        archive_dir: archives # 归档文件夹
        category_dir: categories # 分类文件夹
        code_dir: downloads/code # Include code 文件夹
        i18n_dir: :lang # 国际化（i18n）文件夹
        skip_render: # 跳过指定文件的渲染，您可使用 glob 表达式来匹配路径。
        - README.md
        
        # Writing，文章，这一块一般不需要修改
        new_post_name: :title.md # Hexo 默认以标题做为文件名称，设为 :year-:month-:day-:title.md 可让您更方便的通过日期来管理文章
        default_layout: post # 预设布局，Hexo 有三种默认布局：post、page 和 draft，分别对应不同的路径：source/_posts、source和source/_drafts
        titlecase: false # 把标题转换为 title case
        external_link: true # 在新标签中打开链接
        filename_case: 0 # 把文件名称转换为 (1) 小写或 (2) 大写
        render_drafts: false # 显示草稿，草稿默认不会显示在页面中
        post_asset_folder: false # 启动 Asset 文件夹
        relative_link: false # 把链接改为与根目录的相对位址 
        future: true # 显示未来的文章
        highlight: # 代码块的设置
          enable: true
          line_number: true
          auto_detect: false
          tab_replace:
          
        # Home page setting
        # path: Root path for your blogs index page. (default = '')
        # per_page: Posts displayed per page. (0 = disable pagination)
        # order_by: Posts order. (Order by date descending by default)
        index_generator:
          path: ''
          per_page: 10
          order_by: -date
          
        # Category & Tag，分类 & 标签，这一块一般不需要修改
        default_category: uncategorized # 默认分类
        category_map:
        tag_map:
        
        # Date / Time format，日期 / 时间格式，这一块一般不需要修改
        ## Hexo uses Moment.js to parse and display date
        ## You can customize the date format as defined in
        ## http://momentjs.com/docs/#/displaying/format/
        date_format: YYYY-MM-DD # 日期格式
        time_format: HH:mm:ss # 时间格式
        
        # Pagination，分页，这一块一般不需要修改
        ## Set per_page to 0 to disable pagination
        per_page: 10 # 每页显示的文章量 (0 = 关闭分页功能)
        pagination_dir: page # 分页目录
        
        # Extensions，扩展
        ## Plugins: https://hexo.io/plugins/
        ## Themes: https://hexo.io/themes/
        theme: 3-hexo # 当前主题名称。值为false时禁用主题
        
        # Deployment，部署
        ## 这里是重点，这里是修改发布地址，因为我们前面已经将本地 SSH 密钥信息添加到 Github 设置里面了，所以只要我们电脑里面持有那两个密钥文件就可以无需密码地跟 Github 做同步。
        ## 需要注意的是这里的 repo 采用的是 ssh 的地址，而不是 https 的。分支我们默认采用 master 分支。
        ## Docs: https://hexo.io/docs/deployment.html
        deploy:
          type: git
          repo: git@github.com:tujidelv/tujidelv.github.io.git
          branch: master
        ```
    - 执行如下命令 `hexo clean && hexo g && hexo d`
        ```
        hexo clean          // 清除掉已经生成的旧文件
        hexo generate/g     // 重新生成一次静态文件
        hexo deploy/d       // 使用部署命令部署到 Github 上
        ```
    ![抱歉,图片休息了](af-hexo/af-hexo-004.png "hexo 部署到GitHub")
- 绑定域名
    - 在 source 目录下新建 `CNAME` 文件（文件名叫 CNAME，没有文件后缀的）
        - 以后一些需要放在根目录的资源文件都可以放这里。
        - ***`CNAME` 文件上的内容写你要绑定的域名全称。***
    - 到各自己域名控制台上设置域名解析(以阿里云为例)
        ```
        记录类型：CNAME
        主机记录：www
        解析线路：默认
        记录值：tujidelv.github.io.
        TTL：默认
        ```
- 整合 IntelliJ IDEA 提高效率
    - 为了提交写作效率，建议使用 IDEA 作为 Markdown 编辑工具。
      - IDEA 有各种各样的快捷键支持你的操作
      - IDEA 可以快速地全文检索项目所有的文件
      - 对 JavaScript、CSS、HTML 等常见语言的良好支持，方便修改主题
    - 打开 hexo 项目，将 public 目录设置成 Excluded，这样每次 hexo 新的静态博客后 IDEA 不需要每次都去重建索引。

## 进阶使用

### **编写脚本方便快速部署和本地预览**

- 在 hexo 根目录下新建 hs.sh 和 hd.sh 文件,分别加入如下内容：
    ```
    #!/bin/bash
    hexo clean && hexo g && hexo s
    ```
    ```
    #!/bin/bash
    hexo clean && hexo g && hexo d
    ```
- 在 Git Bash 中分别执行 `./hs.sh` 和`./hd.sh` 来预览和部署。

### **创建[关于我]页面**

- 可以编辑 index.md 文件补充需要显示的内容。
```
Tujide.lv@tujide MINGW64 /e/Ebook/JavaSE/workspace_idea/hexo-blog
$ hexo new page about
INFO  Created: E:\Ebook\JavaSE\workspace_idea\hexo-blog\source\about\index.md
```

### **创建[404]页面**

- 对于 github page 来说，只要在根目录有 404.html，当页面找不到时，就会被转发到/404.html 页面，所以我们只要更改这个页面，就可以实现自定义 404 页面了。
- 但是我们通常会需要与本主题相符的 404 页面，那我们需要以下操作
    1. 进入 Hexo 根目录，输入 `hexo new page 404` ;
    2. 打开刚新建的页面文件，默认在 Hexo 文件夹根目录下 `/source/404/index.md`;
    3. 在顶部插入一行，写上 `permalink: /404`，这表示指定该页固定链接为 `http://"主页"/404.html`。

### **创建[分类&标签]页面**

1. 在 source 目录下新建一个 tags 文件夹
    ```
    $ hexo new page tags
    ```
2. 编辑 index.md 文件，添加 `layout` 选项，后面的值对应的是主题文件夹下 `layout` 目录下第一级的布局文件，例如layout/tags.ejs
    ```
    ---
    title: tags
    date: 2018-11-28 20:23:17
    type: tags
    layout: "tags"
    ---
    ```
3. 编辑主题配置文件，各个主题的显示名称可能不同
    ```
    menu:
      Home: /
      Archives: /archives
      tags: /tags
    ```
4. 最后去看下 hexo 站点配置文件对应的选项名称是否对应
    ```
    # Directory
    tag_dir: tags
    ```
Tips：最重要的是看一下主题文件里有没有标签页或者分类页的布局文件，一般来说都是有的，只是命名和存放的位置可能不同，所以大家要根据实际情况来修改。categories 同理。

### **图片显示**

- 一般分为外链和本地图片，这里介绍本地图片显示的 2 种方法
- 绝对路径，可直接在 source 目录下新建 images 文件夹用于存放图片，在 md 文件中如下引用
    ```
    ![图片不存在时的提示文字](/images/图片.jpg "鼠标箭头放到图片上的提示文字")
    ```
- 相对路径，需先将站点配置文件中的 post_asset_folder 设为 true，然后安装 hexo-asset-image 插件
    ```
    # Writing
    post_asset_folder: true # 启动 Asset 文件夹
    ```
    ```
    Tujide.lv@tujide MINGW64 /e/Ebook/JavaSE/workspace_idea/hexo-blog
    $ cnpm i hexo-asset-image --save
    √ Installed 1 packages
    √ Run 0 scripts
    √ All packages installed (19 packages installed from npm registry, used 2s(network 2s), speed 153.72kB/s, json 18(31.65kB), tarball 315.44kB)
    ```
    ```
     ![图片不存在时的提示文字](MD文件名称/图片.jpg "鼠标箭头放到图片上的提示文字")
    ```
    Tips：此方法在运行 hexo n "xxxx" 来生成 md 文件时，同时会在/source/_posts 文件夹下生成一个与 md 文件同名的文件夹。
    
### **添加 rss 为 blog 提供订阅功能**

1. 安装相关插件
    ```
    $ cnpm install hexo-generator-feed --save
    ```
2. 配置 Hexo 站点配置文件_config.yml(以 3-hexo 为例)
    ```
    # Extensions
    ## Plugins: https://hexo.io/plugins/
    plugin:
    - hexo-generator-feed
    #Feed Atom
    feed:
    type: atom
    path: atom.xml
    limit: 20
    ```
3. 验证配置是否成功
    - 执行 hexo g，查看一下 public 目录下，如果有 atom.xml 文件，则表明配置成功。
4. 修改 Hexo 主题配置文件_config.yml(以 3-hexo 为例)
    ```
    link:
      items:
        rss: /atom.xml
    ```
    
### **添加 sitemap 让搜索引擎收录**

1. 先确认博客是否被收录
    - 在百度或者谷歌上面输入 `site:你的域名` 来判断，如果能搜索到就说明被收录，否则就没有。
2. 创建站点地图文件
    - 您可以通过该文件列出您网站上的网页，从而将您网站内容的组织架构告知 Google 和其他搜索引擎，搜索引擎网页抓取工具会读取此文件，以便更加智能地抓取您的网站。
    1. 安装百度和 google 插件
        ```
        cnpm install hexo-generator-sitemap --save
        cnpm install hexo-generator-baidu-sitemap --save
        ```
    2. 配置 Hexo 站点配置文件_config.yml(以 3-hexo 为例)
        ```
        plugin:
        - hexo-generator-sitemap
        - hexo-generator-baidu-sitemap
        ```
    3. 验证配置是否成功
        - 执行 hexo g，查看一下 public 目录下，如果有 sitemap.xml 文件，则表明配置成功。
3. 让百度收录我们的博客
    - 可以去 [百度站长平台](https://ziyuan.baidu.com/dashboard/index) 进行网站验证和链接提交。
    - 由于百度不会抓取 GitHub Pages 上的站点，这里不进行操作。
4. 让谷歌收录我们的博客
    - 可以去 [Google 站长工具](https://www.google.com/webmasters/tools) 进行站点验证和添加站点地图。
5. 添加 rebots.txt 文件
    - ...
        
### **文章排序及置顶**

- 文章（site.posts）排序默认按照 .md 文件的创建时间排序，而没有按照文章中的 date 排序。这容易导致问题，比如文件丢失通过 Git 还原后创建时间都是一样的。
- 设置排序
    - 方法1：
        >**[@牵猪的松鼠](http://s.amlove.cn/)根据这篇文章写了一个npm插件 [hexo-generator-topindex](https://www.npmjs.com/package/hexo-generator-topindex)
        安装插件命令： `npm install hexo-generator-topindex --save`
    
    - 方法2：直接修改 node_modules/hexo-generator-index/lib/generator.js
        ```javascript
        'use strict';
        var pagination = require('hexo-pagination');
        module.exports = function(locals){
          var config = this.config;
          var posts = locals.posts;
            posts.data = posts.data.sort(function(a, b) {
                if(a.top && b.top) { // 两篇文章top都有定义
                    if(a.top == b.top) return b.date - a.date; // 若top值一样则按照文章日期降序排
                    else return b.top - a.top; // 否则按照top值降序排
                }
                else if(a.top && !b.top) { // 以下是只有一篇文章top有定义，那么将有top的排在前面（这里用异或操作居然不行233）
                    return -1;
                }
                else if(!a.top && b.top) {
                    return 1;
                }
                else return b.date - a.date; // 都没定义按照文章日期降序排
            });
          var paginationDir = config.pagination_dir || 'page';
          return pagination('', posts, {
            perPage: config.index_generator.per_page,
            layout: ['index', 'archive'],
            format: paginationDir + '/%d/',
            data: {
              __index: true
            }
          });
        };
        ```
- 设置置顶
    - 给需要置顶的文章加入top参数，如下
        ```
        ---
        title: Hexo 札记
        date: 2018-12-01 18:25:23
        top: 1
        categories:
        - 应用框架
        tags:
        - hexo
        - nodejs
        ---
        ```
    - 如果存在多个置顶文章，top 后的参数越大，越靠前。

### **使用 gitment 评论系统**

- 简介
    >[Gitment 项目地址](https://github.com/imsun/gitment)
    
    >Gitment 是基于 GitHub Issues 的评论系统。支持在前端直接引入，不需要任何后端代码。可以在页面进行登录、查看、评论、点赞等操作，同时有完整的 Markdown / GFM 和代码高亮支持。尤为适合各种基于 GitHub Pages 的静态博客或项目页面。
- 注意点
    - gitment 有两个配置文件(一个 css 文件和一个 js 文件)，gitment 的开发者用远程连接的方式将两个文件引入 hexo 中，其中的 js 文件中需要访问作者自己搭建的服务器，但是现在作者的服务器不好使了，会导致全都配置好了，但是报错。所以要下载到本地自己的项目中，然后在项目中自己引用这两个文件。
    - 由于主题不同，有的主题的作者在开始设计主题的时候就为用户写好了gitment 的配置代码，只需用户在主题的_config.yml 中开启并填写好配置参数即可，有的主题没有预制好 gitment 的配置代码，就需要用户自己去在样式模板中添加 gitment 代码。
- 安装 gitment 到主题
    - 主题已经集成了 gitment 模块
        ```
        需要博主自己查看一下自己的主题结构，找到 themes 下的 _config.yml 文件，在文件中找到 gitment 模块的相关参数，然后在 enable 选项后面写上 true 即可完成安装过程。
        ```
    - 主题没有集成 gitment 模块
        ```
        将这两个文件下载到本地并引入自己主题的母板中就完成了安装，可根据自己的文件结构放置 css 和 js 文件，然后在模板文件中引用两个文件。
        本质上安装就是在主题模板文件中引入一个 css 和一个 js 文件，集成 gitment 模块的主题只不过是做到了代码分离，让需要配置的参数在_config.yml 中统一配置。
        gitment 作者给出的方法是在给没有集成的主题中的根本引入办法，如果你能看懂主题作者的组织结构，那完全可以给自己的主题集成 gitment 模块。
        
        下载地址：
        https://imsun.github.io/gitment/style/default.css
        https://imsun.github.io/gitment/dist/gitment.browser.js
        ```
        ```
        <link rel="stylesheet" href="https://tujidelv.github.io/css/gitment.css">
        <script src="https://tujidelv.github.io/js/gitment.js"></script>
        ```
- 注册 OAuth application
    - 因为 gitment 是利用了 github 的 issue，所以要注册 OAuth application，来获取配置参数为接下来的配置做准备。
      
    - [点击此处](https://github.com/settings/applications/new)处来注册一个新的 OAuth Application。其他内容可以随意填写，但要确保填入正确的 callback URL（一般是评论页面对应的域名，如 <https://lvzhiqiang.top/>）。
      
    - 注册后会给两个字符串 Client ID 和 Client Secret , 这两个下面配置的时候要用。

- 配置 gitment 到 hexo 主题中
    - 主题已经集成了 gitment 模块
        ```
        只需在 _config.yml 中找到刚才开启 gitment 的那里，依次填入GitHub 用户名、存储评论的仓库地址、Clieent Id、和 Client Secret。
        ```
    - 主题没有集成 gitment 模块
        ```
        在刚才添加的母版中继续添加如下代码，并将以下代码中的四个参数按照提示填好即可。
        ```
        ```
        <div id="comments"></div>
        <script>
        var gitment = new Gitment({
          id: '<%=url %>', // 可选。默认为 location.href
          owner: '你的 GitHub ID(可以是你的 GitHub 用户名，也可以是 Github id，建议直接用 GitHub 用户名就可以)',
          repo: '存储评论的 Github repo(可以是博客下的仓库，也可以新建一个仓库专门存储评论内容)',
          oauth: {
            client_id: '你的 client ID',
            client_secret: '你的 client secret',
          },
        })
        gitment.render('comments')
        </script>
        ```
- 初始化评论
    - 理论上按照以上配置完发布即可看到评论区了。只不过每一个帖子下面的评论区都要点击 `Initialize Comments` 才能开始评论。
    - 根据 gitment 作者的说法，只要博主登录自己的 github 账号然后点击初始化就可以使用了。
- 常见问题解决
    1. 评论时点击登录，登录 GitHub 之后跳转回来的时候不能正常跳回刚才评论那页，每次跳到主页。
        ```
        解决办法：在注册 OAuth application 时的回调地址有问题，尝试写绑定的域名，而不是用 “https://用户名.github.io”
        ```
    2. 报错 `[Error: Comments Not Initialized]`
        ```
        文章作者需要登陆 GitHub 授权后，会显示初始化按钮(注意，不要多点按钮，否则 issues 出现多条一样的)
        点击初始化按钮后，如果正常就会出现"No Comment Yet"
        关于自动初始化所有文章的功能，可以用脚本来执行自动化，有需要的可以详细了解：https://github.com/imsun/gitment/issues/5
        ```
    3. 报错 `[object ProgressEvent]`
        ```
        原因：在母版中调用的 js 文件中，有访问 gitment 作者的服务器代码，而作者的服务器不好使了。
        解决办法： 自己搭建一个服务器（需要有一个 vps 来辅助搭建服务器）
        参考：https://github.com/imsun/gitment/issues/170
        --------------------------------
        1.在服务器中 clone 作者的服务器源码
            git clone https://github.com/imsun/gh-oauth-server.git
        2.进入项目，下载依赖，并启动(如果成功,在项目目录下的 nohup.out 文件中的最后，会提示正在监听 3000 端口)
            npm install && nohup npm start
        3.替换 js 文件中的作者服务器为自己服务器，在作者的 js 文件中搜索以下字符串并将其替换成刚才搭建服务器的地址，如果没有做好端口映射可直接地址加端口也可以。
            https://gh-oauth.imsun.net
        ```
        ```
        推荐几位大佬搭建好的：
            https://bak.smalbox.club
            https://auth.baixiaotu.cc
            https://cors.wenjunjiang.win/?remoteUrl=https://github.com/login/oauth/access_token
            https://github.com/login/oauth/access_token
        ```
    4. 报错 `[Error：validation failed]`
        ```
        原因：
            issue 的标签 label 有长度限制！labels 的最大长度限制是 50 个字符。
        --------------------------------
        解读：
            id: '页面 ID', // 可选。默认为 location.href
            这个id的作用，就是针对一个文章有唯一的标识来判断这篇本章。
            在issues里面，可以发现是根据网页标题来新建issues的，然后每个issues有两个labels（标签），一个是gitment，另一个就是id。
            所以明白了原理后，就是因为id太长，导致初始化失败，现在就是要让id保证在50个字符内。
            对应配置的id为：
            id: '<%= page.title %>'
            如果用网页标题也不能保证在50个字符！
        --------------------------------
        解决办法：
            最后，我用文章的时间，这样长度是保证在50个字符内，完美解决！（避免了文章每次更新标题或路径时，会重新创建一个issue评论的问题。）
            id: '<%= page.date %>'
        ```
    5. gitment 的汉化
        ```
        只需到模板里将原来定义CSS和JS的那两行改成如下即可
            <link rel="stylesheet" href="https://billts.site/extra_css/gitment.css">
            <script src="https://billts.site/js/gitment.js"></script>
        来源：https://github.com/imsun/gitment/issues/104
        ```
    6. 订阅 issue
        ```
        issue 订阅，有新评论时就可以通过邮件提醒，这个功能是把双刃剑，因为有些垃圾订阅邮件骚扰，大家看着用吧。
        ```

### **备份当前项目源码**

- 为防止当前项目源文件丢失，特补充此备份操作。
- 修改 Hexo 项目的 .gitignore 文件，然后通过 IDEA 同步到远程仓库。
    ```
    ### Hexo ###
    .DS_Store
    Thumbs.db
    db.json
    *.log
    node_modules/
    public/
    .deploy*/
    themes/
    
    ### IntelliJ IDEA ###
    .idea/
    *.iml
    ```
![抱歉,图片休息了](af-hexo/af-hexo-005.png "将 hexo 源文件分享到 github")

## 参考链接

- <http://www.youmeek.com/hexo-install/>

## 结束语

- 未完待续...