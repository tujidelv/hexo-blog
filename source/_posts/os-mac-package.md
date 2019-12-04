---
title: MacOS 系统进阶：包管理工具
date: 2019-11-22 15:07:31
categories:
- 操作系统
tags:
- mac
---

## 目录

- [简介](#简介)
- [正篇](#正篇)
- [参考链接](#参考链接)
- [结束语](#结束语)

## 简介

- Mac与Linux都是基于Unix的系统，因此继承了很多Unix的特性，包括软件的编译，安装等。
- 有些操作，命令行或者说脚本的方式效率是远高于GUI界面操作的，这个概念需要用过Unix/Linux做过开发的人会懂，特别是搞运维的。

## 正篇

### **简介Homebrew**

- Homebrew官网：[https://brew.sh/index_zh-cn.html](https://brew.sh/index_zh-cn.html)
    > **1.使用Homebrew安装Apple（或您的Linux系统）没有预装但你需要的东西。**  
**2.Homebrew会将软件包安装到独立目录(/usr/local/Cellar和/usr/local/opt/)，并将其文件软链接至/usr/local/bin。**  
3.Homebrew不会将文件安装到它本身目录之外，所以您可将Homebrew安装到任意位置。  
4.轻松创建你自己的Homebrew 包。  
5.完全基于Git和Ruby，所以自由修改的同时你仍可以轻松撤销你的变更或与上游更新合并。  
6.Homebrew的配方都是简单的Ruby脚本。  
7.Homebrew 使 macOS（或您的 Linux 系统）更完整。使用 gem 来安装 RubyGems、用 brew 来安装那些依赖包。  
**8.“要安装，请拖动此图标......”不会再出现了。使用 brew cask 安装 macOS 应用程序、字体和插件以及其他非开源软件。**  
9.制作一个 cask 就像创建一个配方一样简单。
        
- 同类技术比较：[Homebrew 和 Fink、MacPort 相比有什么优势？](http://www.zhihu.com/question/19862108)
- Homebrew是Mac下的一个包管理工具，类似于Ubuntu的apt-get，CentOS的yum。
    > 它会帮我们下载好源码解压，并且编译执行安装，还会下载各种依赖包，设置好各种配置和参数。  
    这个对程序员来说简直是福音，简单的指令，就能快速安装和升级本地的各种开发环境。

### **安装Homebrew**

1. 将以下命令粘贴至终端安装Xcode command line tools，如果提示已经安装过了那就不用管了。
    > $ xcode-select --install
2. 将以下命令粘贴至终端
    > $ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
3. 测试安装是否成功
    > $ brew doctor
4. 卸载
    > $ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"  
$ rm -rf /usr/local/Homebrew

### **使用Homebrew**

| 命令 | 备注 |
|:---------|:---------|
| brew install wget | 安装指定软件包 |
| brew uninstall wget | 卸载指定软件包，一键静默卸载 |
| brew upgrade wget | 更新指定软件包,如果不加软件名，就更新所有可以更新的软件 |
| brew info svn | 查看指定软件包的各种信息，包括版本源码地址等等 |
| brew search git | 模糊搜索brew支持的软件，如果不加软件名，就会列出所有它支持的软件 |
| brew list | 列出本机通过brew安装的所有软件 |
| brew update | 更新brew软件自身 |
| brew cleanup | 清除下载的各种缓存 |

### **更换brew源为国内源(可选)**

1. 替换brew.git
    > $ cd "$(brew --repo)"  
      $ git remote set-url origin https://mirrors.ustc.edu.cn/brew.git
2. 替换homebrew-core.git
    > $ cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
      $ git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
3. 替换Homebrew Bottles源
    > $ echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bash_profile  
      $ source ~/.bash_profile
      
    > 上面是针对bash用户，对于zsh用户是~/.zshrc文件。
4. 重置为官方源
    > $ cd "$(brew --repo)"  
      $ git remote set-url origin https://github.com/Homebrew/brew.git  
      $ cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"  
      $ git remote set-url origin https://github.com/Homebrew/homebrew-core
      
    > 另外，如果Homebrew Bottles源也被替换了的话，只要在~/.bash_profile配置文件里删除掉对应的命令所在行，并source一下即可。

---  
<https://mirror.tuna.tsinghua.edu.cn/help/homebrew/>  
<https://mirror.tuna.tsinghua.edu.cn/help/homebrew-bottles/>

### **Proxychains4为终端做代理(可选)**

```
1.保证你本地有一个 socks5 到代理工具，不然下面的方法你无法使用。我这里的工具是：Shadowsocks
2.安装 Proxychains4，输入命令：brew install proxychains-ng
3.修改配置文件：vim /usr/local/etc/proxychains.conf
    在配置文件中找到：[ProxyList]（也就是第 111 行的地方），在其下面一行新增一条：socks5 127.0.0.1 1080 # my vps
4.测试：proxychains4 wget www.google.com，如果你能正常下载到 Google 页面，则表示成功了。以后只要在命令前面加个：proxychains4，即可。
5.修改终端配置，让命令更加简洁：
    如果你是 zsh 终端，配置修改：vim ~/.zshrc，添加一行：alias proxy='proxychains4'
    如果你是 bash 终端，配置修改：vim ~/.bash_profile，添加一行：alias proxy='proxychains4'
    修改之后，以后要用 proxychains4 执行穿墙命令的话，那就可以这样写：proxy wget google.com
```

### **简介brew cask**

- brew cask 官网：<https://buyinstagramlikes.io/caskroom>
    ```
    brew cask 是在brew 的基础上一个增强的工具，用来安装Mac上的Gui程序应用包（.dmg/.pkg）, 比如qq、chrome、xun lei等。
    它先下载解压到统一的目录中（/opt/homebrew-cask/Caskroom），省掉了自己去下载、解压、拖拽（安装）等蛋疼步骤，同样，卸载相当容易与干净。
    然后再软链到~/Applications/目录下，这样就能在应用程序里找到它，非常方便。而且还包含很多在 AppStore 里没有的常用软件。
    ```
- brew cask Github：<https://github.com/caskroom/homebrew-cask>

### **安装brew cask**

1. 将以下命令粘贴至终端
    > $ brew install caskroom/cask/brew-cask
3. 测试安装是否成功
    > $ brew cask

### **使用brew cask**

| 命令 | 备注 |
|:---------|:---------|
| brew cask install qq | 下载安装软件 |
| brew cask uninstall qq | 卸载软件 |
| brew cask info qq | 显示这个软件的详细信息，如果已经用cask安装了，也会显示其安装目录信息等 |
| brew cask search qq | 模糊搜索软件，如果不加软件名，就列出所有它支持的软件 |
| brew cask list | 列出本机按照过的软件列表 |
| brew update && brew upgrade brew-cask | 更新cask自身 |
| brew cask cleanup | 清除下载的缓存以及各种链接信息 |

## 参考链接

- <http://www.youmeek.com/mac-homebrew>
- <https://www.zybuluo.com/phper/note/87055>
- <https://maomihz.com/2016/06/tutorial-6>

## 结束语

未完待续...
