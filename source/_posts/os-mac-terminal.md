---
title: MacOS 系统进阶：终端方案iTerm2 + Zsh + Vim
date: 2019-11-24 14:02:26
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

方便前/后端开发人员更好的通过终端解决问题。

## 正篇

### **简介iTerm2**

- iTerm2官网：<https://iterm2.com>
- Mac的默认终端Terminal太难用了，需要一个新的终端来代替它，iTerm2 + Zsh就是其中一种方案。
- iTerm2是Terminal的替代品，是iTerm的后继产品。iTerm2将终端带入了您从未想过一直想要的功能，使其进入了现代时代。

### **安装iTerm2**

- 方法1：官网下载下来是一个zip压缩包，解压出来有一个.app文件(特殊的文件夹)，双击运行即可安装，或是拖到应用程序里面。
- 方法2：直接使用Homebrew进行安装，命令参考`brew cask install iterm2`。
---
- 更改配色方案：Preferences->Profiles->Colors->Color Presets->Import。
    ```
    例如设置配色方案为solarized，iTerm2默认是有带的，如果没有则访问：https://github.com/altercation/solarized，
    在项目中找到solarized/iterm2-colors-solarized 目录，下面有两个文件：Solarized Dark.itermcolors和Solarized Light.itermcolors，
    双击这两个文件就可以把配置文件导入到 iTerm 里了，同时还要再切换到Text标签，把Draw bold text in bold font的勾去掉。
    ```
- 设置背景图片：Preferences->Profiles->Window->Background Image。
- 设置字体：Preferences->Profiles->Text->Font->Chanage Font。
- 将iTerm2设为默认终端。

### **使用iTerm2**

- `智能选中`
    - 在iTerm2中，连续双击选中，连续三击选中整行，连续四击智能选中（智能规则可配置），可以识别网址，引号引起的字符串，邮箱地址等。
    - 在iTerm2中，选中即复制，即任何选中状态的字符串都被放到了系统剪切板中。
- `快速调出窗口(Hotkey Window)`
    - 有时候只是在终端执行几行命令，然后就不再使用它，可是我们还是必须要打开终端，使用完成后关闭它。
    - 使用iTerm2这个功能只要按快捷键，出来虚化的终端，输入命令，然后再把光标放在其他地方自动就消失了。
    - 默认iTerm2没有开启此功能，可去设置快捷键：Preferences->Keys->Hotkey。
- `常用快捷键`

| 快捷键 | 介绍 |
|:---------|:---------|
| Command + / |	按完之后，整个屏幕变成白茫茫的，而光标位置是一个小圆圈清除显示出来 |
| Command + f |	查找，所查找的内容会被自动复制 ,输入查找的部分字符，找到匹配的值按 tab，即可复制，带有补全功能 |
| Command + option + e | 全屏并排展示所有已经打开的标签页，带有可以搜索。 |
| Command + Option + b | 历史回放，类似视频录像的东西，有记录你最近时间内的操作。存放内容设置（Preferences -> Genernal -> Instant Replay）。 |
| Command + Option + 数字 |	切换 Window 窗口 |
| Command + shift + d |	水平分屏 |
| Command + shift + h |	查看剪贴板历史，在光标位置下方会出现一列你输入过的历史记录 |
| Command + Shift + m |	可以保存当前位置，之后可以按Command + Shift + j跳回这个位置。 |
| Command + shift + alt + w | 关闭所有窗口 |

| 快捷键 | 介绍 | 快捷键 | 介绍 |
|:---------|:---------|:---------|:---------|
| Command + 方向键 |	切换标签页 | Control + t |	交换光标和之前的字符 |
| Command + 数字 | 切换到指定数字标签页 | Control + c |	结束当前状态，另起一行 |
| Command + ; |	根据(输入的前缀)历史记录自动补全 | Control + k |	删除从光标到行尾的内容 |
| Command + [ 或 ] | 切换屏幕 | Control + w |	删除光标之前的单词 |
| Command + enter |	进入全屏模式，再按一次返回 | Control + d |	删除光标所在位置的字符 |
| Command + 鼠标单击	| 可以打开文件/文件夹/链接 | Control + h |	删除光标之前的字符 |
| Command + n |	新建新的 Window 窗口 | Control + y |	召回最近用命令删除的文字 |
| Command + t |	新建标签页 | Control + r |	搜索历史命令 |
| Command + w |	关闭当前标签或是窗口 | Control + n |	下一条命令，相当于方向键下 |
| Command + d |	竖直分屏 | Control + p |	上一条命令，相当于方向键上 |
| Command + r |	清屏 | Control + b |	向后移动，相当于方向键左 |
| Control + u |	清空当前行，无论光标在什么位置 | Control + f |	向前移动，相当于方向键右 |
| Control + a |	移动到行首 | Control + e |	移动到行尾 |

### **简介Zsh**

- Zsh官网：<https://www.zsh.org>
    ```
    Mac和一般的Linux发行版默认采用的shell是bash，并不是很好用。
    Zsh是Shell中的一种，由于Zsh配置门槛有点高(需要专门花时间去了解Zsh)，因为这样，用户也就相对少了。
    ```
- oh-my-Zsh官网：<http://ohmyz.sh>
    ```
    直到有一天oh-my-Zsh的作者诞生，他想要整理出一个配置框架出来，让大家直接使用他的这个公认最好的Zsh配置，省去繁琐的配置过程。
    所以，oh-my-Zsh就诞生了，它只是为了让你减少Zsh的配置，然后又可以好好的享受Zsh这个Shell。
    oh-my-Zsh是基于zsh一个扩展工具集，提供了丰富的扩展功能，包括主题配置，插件机制，已经内置的便捷操作。
    ```
- 修改终端配置就变成了：vim ~/.zshrc，而不是：vim ~/.bash_profile。

### **安装Zsh**

1. 安装Zsh
    ```
    $ brew install zsh
    ```
2. 安装oh-my-Zsh
    ```
    $ sh -c "$(wget -qO- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
    ```
    或者
    ```
    $ sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
    ```
    下载完后，会提示你输入当前登录系统的用户密码，输入完成之后就会从bash切换到zsh，如果你没有输入密码直接跳过了，
    可以运行该命令进行手动切换：chsh -s /bin/zsh gitnav(系统用户名),切换完成之后，关掉终端，重新打开终端即可。
3. 设置oh-my-Zsh主题(可选)
    - 主题列表：<https://github.com/ohmyzsh/ohmyzsh/wiki/Themes>
    - oh-my-Zsh自带了一些默认主题，存放在 ~/.oh-my-zsh/themes 目录中。
    - 固定主题：例如在.zshrc文件中修改(或添加)`ZSH_THEME="agnoster"`。
    - 随机主题：`ZSH_THEME="random"`。
4. 安装Zsh插件(可选)

### **使用Zsh**

- 不区分大小写智能提示，按Tab键进行提示。
- 按下tab键显示出所有待选项后，再按一次tab键，即进入选择模式，按tab/shift + tab 切向下/上一个选项，ctrl+f/b/n/p 可以向前后左右切换。
- kill + 空格键 + Tab键，列出运行的进程，要杀哪个进程不需要再知道PID了。
    - zsh提供了让你知道PID的方法：例如输入kill vim，再按下tab，会变成：kill 5643
- ls **/*，分层级地列出当前目录下所有文件及目录，并递归目录
    - ls *.png 查找当前目录下所有 png 文件
    - ls **/*.png 递归查找
- zsh的目录跳转很智能，你无需输入cd就可直接输入路径即可。比如：..表示后退一级目录，../../表示后退两级，依次类推。
- 在命令窗口中输入：d，将列出当前session访问过的所有目录，再按提示的数字即可进入相应目录。
- 给man命令增加结果高亮显示，可编辑配置文件：vim ~/.zshrc，增加下面内容
    ```
    # man context highlight
    export LESS_TERMCAP_mb=$'\E[01;31m'       # begin blinking
    export LESS_TERMCAP_md=$'\E[01;38;5;74m'  # begin bold
    export LESS_TERMCAP_me=$'\E[0m'           # end mode
    export LESS_TERMCAP_se=$'\E[0m'           # end standout-mode
    export LESS_TERMCAP_so=$'\E[38;5;246m'    # begin standout-mode - info box
    export LESS_TERMCAP_ue=$'\E[0m'           # end underline
    export LESS_TERMCAP_us=$'\E[04;38;5;146m' # begin underline
    ```
- 常用插件
    ```
    git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
    vim ~/.zshrc，在 plugins 里面换行，分别添加：zsh-autosuggestions，zsh-syntax-highlighting
    ```

### **简介Vim**

略

### **安装Vim**

brew install vim

### **使用Vim**

下载配置：curl https://raw.githubusercontent.com/wklken/vim-for-server/master/vimrc > ~/.vimrc


## 参考链接

- <http://www.youmeek.com/mac-iterm2>

## 结束语

未完待续...
