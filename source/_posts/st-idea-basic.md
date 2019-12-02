---
title: IDEA 札记：基础
date: 2018-12-01 16:51:07
categories:
- 软件工具
tags:
- idea
---

# IDEA 札记：基础

## 目录

- [简介](#简介)
- [安装、基础环境介绍](#安装、基础环境介绍)
- [开始使用](#开始使用)
- [常用设置介绍](#常用设置介绍)
- [其他设置介绍](#其他设置介绍)
- [参考链接](#参考链接)
- [结束语](#结束语)

## 简介

- `JetBrains 公司介绍`
    - 维基百科：
        >JetBrains 是一家**捷克**的软件开发公司，该公司位于捷克的布拉格，并在俄罗斯的圣彼得堡及美国麻州波士顿都设有办公室，该公司最为人所熟知的产品是 Java 编程语言开发撰写时所用的集成开发环境：IntelliJ IDEA。
         
        >JetBrains 成立于 2000 年，是一家**私人持股**的公司，该公司的合伙创办人有：Sergey Dmitriev、Eugene Belyaev 及 Valentin Kipiatkov。
         
        >截至 2017 年 6 月，该公司共发布了 24 款开发工具与及相关产品。
    - JetBrains 公司旗下其他产品，包括集成开发环境、插件、编程语言、团队工具、其他等等。
        ```
        PhpStorm：主要用于开发PHP
        RubyMine：主要用于开发Ruby/Rails
        PyCharm：主要用于开发Python
        AppCode：主要用于开发Objective-C/Swift,xcode的替代者
        CLion：主要用于开发C/C++
        WebStorm：主要用于开发JavaScript、HTML5、CSS3等前端技术	
        DataGrip：主要用于开发数据库和SQL
        Rider：主要用于开发.NET
        GoLand：主要用于开发Go(区块链等)
        Android Studio：主要用于开发Android（Google基于IntelliJ IDEA社区版进行迭代所以也姑且算上）
        ```
- `Intellij IDEA 介绍`
    - 官网：<https://www.jetbrains.com/idea/>					
    - 新特性列表：<https://www.jetbrains.com/idea/whatsnew>
    - 详细使用文档：<https://www.jetbrains.com/help/idea/meet-intellij-idea.html>
    - IntelliJ IDEA 在 2015 年 06 月官网主页是这样介绍自己的：
        >Excel at enterprise, mobile and web development with Java, Scala and Groovy, with all the latest modern technologies and frameworks available out of the box.
        
        >简明翻译：IntelliJ IDEA主要用于支持Java、Scala、Groovy 等语言的开发工具，同时具备支持目前主流的技术和框架，擅长于企业应用、移动应用和Web应用的开发。
        
    - 如果用一句话来形容 IntelliJ IDEA，我会说：***IntelliJ IDEA 是目前所有 IDE 中最具备沉浸式的 JVM IDE，没有之一。***
- `IDEA 主要功能介绍`

| 安装插件后支持 | SQL 类 | 基本JVM |
|:------:|:------:|:------:|
|PHP|PostgreSQL|Java|
|Python|MySQL|Groovy|
|Ruby|Oracel||
|Scala|SQL Server||
|Kotlin|||
|Clojure|||

| 支持的框架 | 额外支持的语言代码提示 | 支持的容器 |
|:------:|:------:|:------:|
|Spring MVC|HTML5|Tomcat|
|GWT|CSS3|TomEE|
|Vaadin|SASS|WebLogic|
|Play|LESS|JBoss|
|Grails|JavaScript|Jetty|
|Web Services|CoffeeScript|WebSphere|
|JSF|Node.js||
|Struts|ActionScript||
|Hibernate|||
|Flex|||

- `IDEA 主要优势(相较于 Eclipse 而言)`
    - 强大的整合能力，例如 Git、Maven、Spring 等工具和框架。
    - 提示功能的快速、便捷	。
    - 提示功能的范围广。
    - 好用的快捷键和代码模板。
    - 精准搜索。

## 安装、基础环境介绍

- `Windows 下安装`
    - 首次安装
        - IDEA 的安装是非常简单的，不需要做过多的选择，可以说简单到都是 Next 即可。
        - 中间会有个创建快捷图标，建议勾上，方便定位 IDEA 的安装目录；确让是否与 java,groovy,kt 文件关联,建议不勾选。
    - 已有旧版本安装新版本
        - 在小版本迭代中建议是卸载掉旧版本的，然后再进行新版本安装，因为小版本迭代一般都是 Bug 的修复，保留旧版本没有多大意义。
        - 在大版本迭代中建议是保留旧版本，也就是不勾选之前版本，IDEA 是支持一台电脑装多个版本的。
    - 卸载
        - 有 2 个选项，1 是删除缓存和本地历史记录，2 是删除个性化设置和插件。
    - 安装总结
        - 从安装上来看，IDEA 对硬件的要求看上去不是很高，可是实际在开发中其实并不是这样的，因为 IDEA 执行时会有大量的缓存、索引文件。
        - 如果你正在使用 Eclipse/MyEclipse，想通过 IDEA 来解决计算机的卡、慢等问题，这基本上是不可能的，本质上你应该对自己的硬件设备进行升级。
        - 安迪-比尔定律：IT界三大定律之一(摩尔定律、反摩尔定律、安迪-比尔定律)。
- `安装目录介绍`
    ```
    bin：执行文件和启动参数等
      idea.exe：32位IDEA的启动文件，IDEA安装完默认发送到桌面的也就是这个执行文件的快捷方式
      idea.exe.vmoptions：32位IDEA的VM配置文件
      idea64.exe：64位IDEA的启动文件，要求必须电脑上装有JDK64位版本，64位的系统也是建议使用该文件
      idea64.exe.vmoptions：64位IDEA的VM配置文件
      idea.properties：IDEA的属性配置文件
    help：快捷键文档
    jre64：64位java运行环境
    lib：IDEA依赖的类库
    license：各个插件的许可信息
    plugin：插件
    ```
    ```
    可手动复制这2个文件到config根目录下再修改相关参数。
    idea64.exe.vmoptions：
        -Xms128m：设置初始的内存数,增加该值可以提高java程序的启动速度(16G内存的机器可尝试设置为-Xms512m)
        -Xmx750m：设置最大内存数,增加该值可以减少垃圾回收的频率,进而提高程序性能(16G内存的机器可尝试设置为-Xmx1500m)
        -XX:ReservedCodeCacheSize=240m：设置保留代码占用的内存容量,增加该值可以减少没有用的代码被回收的频率,进而提高程序性能(16G内存的机器可尝试设置为-XX:ReservedCodeCacheSize=500m)
    idea.properties：
        idea.config.path=${user.home}/.IntelliJIdea/config：该属性主要用于指向IDEA的个性化配置目录，默认是被注释，这里需要特别注意的是斜杠方向，这里用的是正斜杠。
        idea.system.path=${user.home}/.IntelliJIdea/system：该属性主要用于指向IDEA的系统文件目录。如果你的项目很多，则该目录会很大，如果你的C盘空间不够的时候，还是建议把该目录转移到其他盘符下。
        idea.max.intellisense.filesize=2500：该属性主要用于提高在编辑大文件时候的代码帮助。IDEA在编辑大文件的时候还是很容易卡顿的。
        idea.cycle.buffer.size=1024：该属性主要用于控制控制台输出缓存。有遇到一些项目开启很多输出，控制台很快就被刷满了没办法再自动输出后面内容，这种项目建议增大该值或是直接禁用掉，禁用语句(=disabled) 
    ```
    Tips：***强烈推荐使用 IDEA 菜单中的 `Help->Edit Custom VM Options`和`Help->Edit Custom Properties` 进行参数个性化配置；而不是直接修改安装目录中的该文件,因为 IDEA 升级/重装后可能会导致修改完全失效！***。
- `设置目录介绍`
    - IDEA 各种配置的保存目录。这个设置目录有一个特性，就是你**删除掉整个目录之后，重新启动 IDEA 会再自动帮你生成一个全新的默认配置**，所以很多时候如果你把 IDEA 配置改坏了，没关系，删掉该目录，一切都会还原到默认。
    - config
        - IDEA 的个性化化配置目录，或者说是整个 IDE 设置目录，此目录可看成是最重要的目录，没有之一。安装新版本的 IDEA会 自动扫描硬盘上的旧配置目录，指的就是该目录。
        - 这个目录主要记录了：**IDE 主要配置功能、自定义的代码模板、自定义的文件模板、自定义的快捷键、Project 的 tasks 记录等等个性化的设置**。
    - system   
        - IDEA 的系统文件目录，是 IDEA 与开发项目之间的一个桥梁目录。
        - 这个目录主要记录了：**缓存、索引、容器文件输出等等**，虽然不是最重要目录，但是也是最不可或缺目录之一。
- `首次运行向导`
    1. 是否导入已有设置
        - 首次启动后，会弹出对话框，选择不导入已有的设置。
    2. 激活
    3. 设置主题
        - 这里根据个人喜好进行选择，也可以选择跳过，后面在 settings 里也可以再设置主题等。
    4. 设置插件
        - 设置 IDEA 中的各种工具/插件，可以选择自定义设置、删除，或者安装本身不存在的插件（比如：支持 Scala 的插件），这里不设置，后面也可以通过界面菜单栏的 settings 行设置。
        
## 开始使用

- `首次打开`
    - Create New Project：创建一个新的工程。
    - Import Project：导入一个现有的工程。**适合导入本身不是idea工程**(包括Eclipse工程，Maven项目，Gradle项目或者直接从源代码创建工程)。
    - Open：打开一个已有工程。**适合打开本身是idea工程**。
    - Check out from Version Control：可以通过服务器上的项目地址 Checkout GitHub 上面项目或其他 Git 托管服务器上的项目。
- `设置显示常见的视图`
    - View->Toolbar或**View-Appearance>Toolbar**，勾选工具条。
    - View->Tool Buttons或**View-Appearance>Tool Window Bars**，勾选按钮组。
- `创建工程`
    - 创建 Java Project/Module
        1. 选择指定目录下的 JDK 作为 Project/Module SDK。
        2. 可以勾选 `Create project from template` 根据模板快速创建项目，也可以不勾选手动创建。
            ```
            Command Line App：会自动创建一个带有main方法的类
            Java Hello World：会自动创建一个带有main方法的并且会打印输出Hello World的类
            ```
        3. 在 `More Settings` 中,Project format(项目格式文件)主要提供两种方式。
            ```
            .idea (directory based)：创建项目的时候自动创建一个.idea的项目配置目录来保存项目的配置信息，这是默认选项
            .ipr (file based)：创建项目的时候自动创建一个.ipr的项目配置文件来保存项目的配置信息
            ```
    - 创建静态 JavaWeb Project/Module
        - 省略...
    - 创建动态 JavaWeb Project/Module
        1. 选择指定目录下的 JDK 作为Project/Module SDK，同时勾选 `JaveEE->Web Application`。
        2. 配置 Tomcat
            ```
            1.Run->Edit Configurations->"+"->Tomcat Server->Local
            2.Server
                - 可配置应用服务器路径,VM参数,JRE,端口等
                    - On Update action：当我们按Ctrl + F10进行容器更新的时候，可以根据我们配置的这个事件内容进行容器更新。其中我选择的Update classes and resources事件是最常用的，表示我们在按Ctrl + F10进行容器更新的时候，我们触发更新编译的类和资源文件到容器中。在Debug模式下，这个也就是所谓的HotSwap。只是这种热部署没有JRebel插件那样好用
                    - On frame deactivation：当我们切换IDEA到浏览器的时候进行指定事件更新，这个一般是因为Web开发的时候，我们需要经常在IDEA和各个浏览器之间来回切换测试，所以才有了这种需求。IDEA是为了帮我们在做这种无聊切换的时候做一些指定事情。当然了，如果切换过于频繁，这个功能还是很耗资源的，所以我设置的是Do nothing表示切换的时候什么都不做
            3.Deployment
            	- 添加要运行的项目及设置访问名称
            d.Logs
            e.Code coverage
            f.Startup/Connection
            ```
- `工程界面展示`
    - 工程下的 src 类似于 Eclipse 下的 src 目录，用于存放代码。
    - 工程下的 .idea(Project 的配置文件目录)和 .iml(Module 的配置文件)都是 IDEA 工程特有的，类似于 Eclipse 工程下的 .settings、.classpath、.project 等。
- `创建 package 和 class`
    - 在 src 目录下创建一个 package，接着在包下 new-class，可以直接创建 `Class、Interface、Enum、Annotation` 等常见类型文件。
    - 然后在下拉框中选择创建的结构的类型，接着在类 HelloWorld 里声明主方法，输出 helloworld，完成测试。
    
    Tips1：**在没有文件的情况下包目录默认是连在一起的，不方便看目录层级关系,可点击 Project 窗口的齿轮处，在弹出的菜单中去掉 `Compact Empty Middle Packages`**。
    Tips2：**IDEA 是一个没有 Ctrl + S 的 IDE，所以每次修改完代码只要管着运行或者调试即可，无需担心保存或者丢失代码。**
- `创建模块`
    - 在 Eclipse 中有 Workspace(工作空间)和 Project(工程)的概念,在 IDEA 中只有 Project(工程)和 Module(模块)的概念。
        - <https://www.jetbrains.com/help/idea/migrating-from-eclipse-to-intellij-idea.html>
        ```
        IDEA 官网说明：
        	An Eclipse workspace is similar to a project in IntelliJ IDEA
        	An Eclipse project maps to a module in IntelliJ IDEA
        翻译：
        	Eclipse中workspace相当于IDEA中的Project
        	Eclipse中Project相当于IDEA中的Module
        ```
    - 从 Eclipse 转过来的人总是下意识地要在同一个窗口管理 n 个项目，这在 IDEA 是无法做到的。IDEA 提供的解决方案是打开多个项目实例，即打开多个项目窗口(一个 Project 打开一个 Window 窗口)。
    - 在 IDEA 中 Project 是最顶级的级别，次级别是 Module。即一个 Project 是由一个或多个 Module 组成。
        - 目前主流的大型项目都是分布式部署的，结构都是类似这种多 Module 结构。这类项目一般是这样划分的，比如：core Module、web Module、plugin Module、solr Module 等等，模块之间彼此可以相互依赖。
        - 通过这些 Module 的命名也可以看出，他们之间都是处于同一个项目业务下的模块，彼此之间是有不可分割的业务关系的。
    - 相比较于多 Module 项目，小项目就无需搞得这么复杂。只有一个 Module 的结构 IDEA 也是支持的，并且 IDEA 创建项目的时候，默认就是单 Module 的结构的,此时 Project 目录和 Module 目录是同一个。
- `删除模块`
    1. 选中相应的模块右键 `Open Module Setting`，点击减号，逻辑删除模块。
    2. 再次选中相应的模块右键 `Delete`，物理删除模块。
- `查看/修改项目配置(File->Project Structure)`
    - Project
        ```
        Project name：项目名称
        Project SDK（Software Development Kit）：JDK配置
        	点击"Edit"按钮,进入SDK的统一管理处,加号可以添加新SDK；减号可以删除光标所选的SDK
        	在开发Java项目过程中，由于IDEA支持管理多个JDK，所以你完全不用担心你系统上不同项目需要不同JDK
        Project language level：编译级别
        	限定项目编译检查时最低要求的JDK特性
        	我们使用JDK8的时候，我们只能向下兼容JDK8及其以下的特性，所以只能选择8及其以下的language level。当项目使用的是JDK8，但是代码却最多使用了JDK7的特性的时候我们可以选择7 - Diamonds，ARM，multi-catch etc
        Project compiler output：class输出路径配置
        ```
    - Modules
        ```
        Sources：可以针对Module选择其他language level，默认选择的是Project language level
        	Sources：一般用于标注类似src这种可编译目录。有时候不单单项目的src目录要可编译，还有其他一些特别的目录也要作为可编译的目录，就需要对该目录进行此标注。只有Sources这种可编译目录才可以新建Java类和包
        	Tests：一般用于标注可编译的单元测试目录。在规范的maven项目结构中，顶级目录是src，maven的src我们是不会设置为Sources的，而是在其子目录main目录下的java目录，我们会设置为Sources。而单元测试的目录是src-test-java，这里的java目录我们就会设置为Tests，表示该目录是作为可编译的单元测试目录。从这一点也可以看出IDEA对maven项目的支持是比较彻底的
        	Resources：一般用于标注资源文件目录。在maven项目下，资源目录是单独划分出来的，其目录为：src-main-resources，这里的resources目录我们就会设置为Resources，表示该目录是作为资源目录。资源目录下的文件是会被编译到输出目录下的
        	Test Resources：一般用于标注单元测试的资源文件目录。在maven项目下，单元测试的资源目录是单独划分出来的，其目录为：src-test-resources，这里的resources目录我们就会设置为Test Resources，表示该目录是作为单元测试的资源目录。资源目录下的文件是会被编译到输出目录下的								
        	Excluded：一般用于标注排除目录。被排除的目录不会被IDEA创建索引，相当于被IDEA废弃，该目录下的代码文件是不具备代码检查和智能提示等常规代码功能
        									
        	注：被标注的目录会在右侧有一个总的概括。值得一提的是classes虽然是Excluded目录，但是由于它的特殊性且不可编辑，所以不显示在这里
        	注：如果要去掉目录的标注含义，可以点击打叉按钮进行删除
        Dependencies：可以针对Module选择其他SDK，默认选择的是Project SDK,以及管理项目的所有依赖包
        	依赖包支持五种操作：
        		加号，表示可以引入新依赖包
        		减号，表示可以去除对应的依赖包
        		向上箭头，表示依赖包可以向上移动位置。依赖包越上面的表示在项目加载的时候越是优先，所以对于同一个依赖包，不同版本，依赖顺序不同，结果也可能会是大不相同的
        		向下箭头，表示依赖包可以向下移动位置，原因同上
        		笔，表示可以编辑依赖包的名称和路径
        ```
    - Libraries
        ```
        点击"+"号,如java可以把项目的lib作为一个总的依赖包放置到项目Libraries中,这样当我们项目需要添加新依赖包时，我们只要放置在lib目录下即可自动被项目引入，原因就是我们这里引的是目录，而不是一个一个依赖包
        ```
    - Facets
        ```
        点击"+"号-,如web可以配置web.xml文件,其他框架配置文件也是如此
        ```
    - Artifacts
        ```
        点击"+"号-,如Web Application:Exploded->From Modules...,修改Output directory输出目录,配置一个war包展开的输出结构
        ```
        
## 常用设置介绍
通过 `File->Settings` 或者 `Ctrl+Alt+S` 进入。
- `Appearance & Behavior(外观和行为)`
    - **设置主题**
        ```
        Appearance,在'UI Options'中选择'Theme'的值
        ```
    - **设置窗体及菜单的字体及字体大小**
        ```
        Appearance,在'UI Options'中修改'Overrid default fonts by(not recommended)'的值
        ```
    - **设置内存使用情况显示**
        ```
        Appearance,在'Window Options'中勾选'Show memory indicator'
            点击内存值后可以进行部分内存的回收	
        ```
    - **设置启动 IDEA 时可以选择最近打开的某个项目**
        ```
        System Settings,在'Shartup/Shutdown'中选择'Confirm application exit'	
            启动IDEA时,默认会打开上次使用的项目,如果只有一个项目的话,该功能还是很好用的,如果有多个项目的话,建议可以选择最近打开的某个项目
        ```
    - **设置另一个项目窗口的打开方式**
        ```
        System Settings,在'Project Opening'中选择	
            Open project in new window 每次都使用新窗口打开
            Open project in the same window 每次都替换当前已打开的项目，这样桌面上就只有一个项目窗口
            Confirm window to open project in 每次都弹出提示窗口，让我们选择用新窗口打开或是替换当前项目窗口
        ```
    - **设置取消更新**
        ```
        System Settings->Updates,去掉'Automatically check updates for'选中
        ```
- `Keymap(快捷键)`
    - ***详见高级篇***
    - **设置基础代码提示、补充快捷键生效**
        ```
        搜索Basic,将Ctrl+空格改为Ctrl+,
        ```
- `Editor(编辑器)`
    - **设置鼠标滚轮控制代码字体大小**
        ```
        General,在'Mouse'中勾选'Change font size with Ctrl+Mouse Wheel'
        ```
    - **设置自动导包功能**
        ```
        General->Auto Import,在'Java'中'Insert imports on paste'选择All
            Add unambiguous imports on the fly：书写代码时自动导入需要用到的包
            Optimize imports on the fly：自动帮我们优化导入的包,比如去掉一些没有用到的包
        ```
    - **设置显示行数及方法间的分隔线**
        ```
        General->Apperance,勾选'Show method separators'和'Show line numbers'
        ```
    - **设置显示空格和Tab**
        ```
        General->Apperance,勾选'Show whitespaces'
        ```
    - **设置忽略代码提示区分大小写**
        ```
        General->Code completion,去掉'Match case'选中
        ```
    - **设置取消单行显示 tabs 的操作**
        ```
        General->Editor Tabs,去掉'Show tabs in one row'选中
            在打开很多文件的时候，IDEA默认是把所有打开的文件名Tab单行显示的。多行效率比单行高，因为单行会隐藏超过界面部分Tab，这样找文件不方便
        ```
    - **设置主题字体**
        ```
        Font,也可以去Editor->Color Scheme->Color Scheme Font/Console Font中分别设置编辑区和控制台的字体
        ```
    - **设置 Java 代码的单行注释跟随在代码的头部**
        ```
        Code Style->Java,切换到Code Generation,在'Comment Code'中去掉'Line comment at first column'选中
            默认IDEA对于Java代码的单行注释是把注释的斜杠放在行数的最开头，个人觉得这样的单行注释非常丑，整个代码风格很难看，所以一般会设置为单行注释的两个斜杠跟随在代码的头部
        ```
    - **设置项目文件编码**
        ```
        File Encodings,对'Global Encoding','Project Encoding'和'Properties Files'进行设置,建议都修改为UTF-8
            Transparent native-to-ascii conversion主要用于转换ascii，一般都要勾选，不然Properties文件中的注释显示的都不会是中文
        ```
    - 设置鼠标悬浮提示
        ```
        General,在'Other'中勾选'Show quick documentation on mouse move'
        ```
    - 设置 Ctrl+E 弹出层显示最近文件的个数
        ```
        General,在'Limits'中修改'Recent files limit'
        ```
    - 设置指定代码类型进行默认折叠或是展开
        ```
        General->Code Folding,勾选上的表示该类型的代码在文件被打开的时候默认是被折叠的，去掉勾选则反之
        ```
    - 设置打开的文件 Tab 个数
        ```
        General->Editor Tabs,在'Closing Policy'中调整'Tab limit'的值
            当打开的文件超过该个数的时候，早打开的文件会被新打开的替换
        ```
    - 设置编辑区主题
        ```
        Color Scheme,如果想要更多的主题效果的话，可以到如下的网站下载,对应文件如何安装请查看网站对应的Help页面，都有详细说明的
            https://www.riaway.com/theme.php
            http://color-themes.com/?view=index
        导入主题(方式1)：
            File–>import setttings–>选中下载的主题jar文件–>一路确认–>重启。重启以后，新主题会自动启用。如果没有启用，可去Editor->Color Scheme选中
        导入主题(方式2)：
            Editor->Color Scheme->齿轮图标->Import Scheme->Intellij IDEA color scheme or settings
        ```
	- 修改代码中注释的字体颜色
        ```
        Color Scheme->Language Defaults,选择需要修改的地方进行设置
            Doc Comment：修改文档注释的字体颜色
            Block comment：修改多行注释的字体颜色
            Line comment：修改单行注释的字体颜色
        ```      
    - 设置开启自动生成 serialVersionUID
        ```
        Inspections,在'Java'->'Serialization'中勾选'Serializable class without serialVersionUID'
            在已经继承了Serializable接口的类名上，把光标放在类名上（必须这样做），按 Alt + Enter，即可提示帮你生成serialVersionUID功能
        ```    
    - 修改类头的文档注释信息
        ```
        File and Code Templates,在'Includes'中选择'File Header'修改
        ```
- `Plugins(插件)`
    - ***详见高级篇***
- `Version Control(版本控制)`
    - ***详见高级篇***
- `Build,Execution,Deployment(构建,执行,部署)`
    - **设置自动编译(不建议,太耗资源)**
        ```
        Compiler,勾选'Build project automatically'和'Compile indepent modules in parallel'
        ------------------------------------------
        构建就是以我们编写的java代码、框架配置文件、国际化等其他资源文件、JSP 页面和图片等资源作为“原材料”，去“生产”出一个可以运行的项目的过程。
        相比较于Eclipse的实时自动编译，IDEA的编译更加手动化,IDEA也支持通过设置开启实时编译，但是不建议，因为太占资源了
        ------------------------------------------
        编译方式介绍(3种)：
            Recompile：对选定的目标（Java类文件），进行强制性编译，不管目标是否是被修改过。
            Rebuild：对选定的目标（Project），进行强制性编译，不管目标是否是被修改过，由于Rebuild的目标是整个Project，所以Rebuild每次花的时间会比较长。
            Build(使用最多的编译操作)：对选定的目标（Project或Module）进行编译，但只编译有修改过的文件，没有修改过的文件不会编译，这样平时开发大型项目才不会浪费时间在编译过程中。	
        ------------------------------------------
        编译器的设置和选择：			
            Compiler下：设置编译heap大小，默认是700，建议使用64位的用户，在内存足够的情况下，建议改为1500 或以上。如果你在编译的时候出错，报：OutOfMemoryError，一般也是要来改这个地方
            Compiler->Excludes下：开发过程中，某一个包目录的文件编译无法通过，但是我们又不急着改，那我们就可以考虑把该包加入到排除编译列表中，则项目就可以运行起来
            Compiler->Java Compiler下：IDEA支持常见的几种编译器：Javac、Eclipse、Ajc等。默认是Javac，也推荐使用Javac
                Project bytecode version 针对项目字节码编译版本，一般选择的是当前项目主JDK的版本。
                Per-module bytecode version 可以针对Project下各个Module的特殊需求单独设置不同的bytecode version，前提是电脑上必须有安装对应的JDK版本。
        ```
    - ***断点调试***
        - 详见高级篇
    - ***部署应用到服务器***
        ```
        1.配置服务器信息
            Deployment,进行服务器信息的配置,包括服务器地址和权限认证,并且在Mapping选项卡完成本地工程与服务器路径的映射。
        2.配置Maven打包插件
            以SpringBoot应用为例,需要配置"spring-boot-maen-plugin"插件,这样执行"mvn package"命令时,即可得到一个可运行的Jar。
        3.部署Jar包
            在"target"目录下找到上一步生成的可执行jar文件右键,依次找到"Deployment->Upload to XXX",就可以上传到远程服务器上。
        4.启动应用
            在菜单栏"Tools->StartSSH session"中开启远程服务器的终端,在IDEA下方可以执行远程指令。
                也可以在"Tools->Deployment->Browse Remote Host"中可视化地浏览服务器上的文件列表,检查应用是否部署成功。
            在远程终端中,找到对应的fatjar,执行"java -jar xxx.jar",便完成了整个部署流程。
        可考虑别一种方案：阿里Cloud Toolkit插件            
        ```
- `Languages & Frameworks(语言和框架)`
- `Tools(工具集)`
    - 设置默认浏览器
        ```
        Web Browsers,在'Default Browser'中选择'Custom path'并选择程序路径
            如果个人有专属的测试浏览器，希望默认从控制台输出的链接是用测试浏览器打开，就可以这样设置
        ```
    - **设置默认终端**
        ```
        Terminal,在'Application settings'中将'Shell path'设置成Git Bash
        可能会出现乱码,可修改Git的安装目录下的etc目录下bash.bashrc文件，在最后一行添加：			
            # 解决IDEA下的terminal中文Unicode编码问题		
            export LANG="zh_CN.UTF-8"
            export LC_ALL="zh_CN.UTF-8"
        ```

## 其他设置介绍

- `设置为省电模式`
    ```
    File->Power Save Mode
        开启这种模式之后IDEA会关掉代码检查和代码提示等功能。所以一般也可认为这是一种阅读模式，如果你在开发过程中遇到突然代码文件不能进行检查和提示，可以来看看这里是否有开启该功能。
    ```
- `清除缓存和索引`
    ```
    File->Invalidate Caches/Restart,一般建议点击'Invalidate and Restart',这样会比较干净,本质也就是去删除C盘下的system目录下的对应的文件而已
    -------------------------------------                          
    清除索引和缓存会使得IDEA的Local History丢失,如果你项目没有加入到版本控制，而你又需要你项目文件的历史更改记录，清除前最好备份下LocalHistory目录。										
    如果你遇到了因为断电、蓝屏引起的强制关机等导致了索引、缓存坏了以至于项目打不开，那也建议你可以直接删除system目录，一般这样都可以很好地解决你的问题。										
    -------------------------------------    
    IDEA首次加载项目的时候，都会创建索引，而创建索引的时间跟项目的文件多少成正比;IDEA的缓存和索引主要是用来加快文件查询，从而加快各种查找、代码提示等操作的速度
    ```
- `设置 Project 项目的一些配置为模板`
    ```
    File->Other Settings->Setting for New Projects...
        对于默认编码、编译版本、Maven 本地库路径等等,可以被当做一个标准的IDE设置模板保存起来,下次打开新的Projec 就会以这个IDE设置进行
    ```
- `自带模拟请求工具 Rest Client`
    ```
    Tools->HTTP Client->Test RESTful web service,在开发时用来模拟请求是非常好用的
    ```
- `生成 javadoc`
    ```
    Tools->Generate JavaDoc,支持对project,module,file
        Locale(输入语言类型)：zh_CN
        Other command line arguments：-encoding UTF-8 -charset UTF-8
    ```
- `设置代码水平或垂直显示`
    ```
    文件tab页上,右键选择Split Vertically(垂直分屏)和Split Horizontally(水平分屏),可设置快捷键
        一般在对大文件进行修改的时候，有些修改内容在文件上面，有些内容在文件下面，如果来回操作可能效率会很低，用此方法就可以好很多
    ```
- `设置代码检查等级`
    ```
    右下角图标。IDEA对于编辑大文件并没有太大优势，很卡，原因就是它有各种检查，这样是非常耗内存和CPU的，所以在编辑大文件的时候为了能加快大文件的读写，我一般会暂时性设置为None
        Inspections：为最高等级检查，可以检查单词拼写，语法错误，变量使用，方法之间调用等
        Syntax：可以检查单词拼写，简单语法错误
        None：不设置检查
    ```
  
## 参考链接

- <https://github.com/judasn/IntelliJ-IDEA-Tutorial>

## 结束语

- 未完待续...
