---
title: MacOS 系统进阶：开发环境Java + Node + Docker
date: 2019-11-24 17:20:38
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

记录从Windows切换到Mac环境IDE的搭建。

## 正篇

### **Git**

- 官网下载：<http://git-scm.com/download/mac>
- 安装过程和 Windows 没啥区别，都是下一步下一步。
- IDEA对Git的支持很好，也不需要额外配置什么，IDEA的Git操作都很便捷，强烈使用IDEA作为Git的GUI操作工具。
- `Homebrew方式(推荐)`：brew install git

### **JDK**

- 官网下载 JDK7：<http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html>
- 官网下载 JDK8：<http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html>
- 安装过程和 Windows 没啥区别，都是下一步下一步，只是比 Windows 简单，连安装路径都不需要改而已。
    ```
    # JDK 1.8
    export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_231.jdk/Contents/Home
    export JRE_HOME=$JAVA_HOME/jre
    export PATH=$JAVA_HOME/bin:$PATH
    export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
    ```
- 卸载
    ```
    sudo rm -rf /Library/Internet\ Plug-Ins/JavaAppletPlugin.plugin
    sudo rm -rf /Library/PreferencePanes/JavaControlPanel.prefPane
    sudo rm -rf /Library/Java/JavaVirtualMachines/jdk1.8.0_231.jdk
    ```
    
### **IDEA**

- 官网下载：<http://www.jetbrains.com/idea>
- 安装过程和 Windows 没啥区别，都是下一步下一步。
    ```
    如果启动Tomcat的时候报：Permission denied，你则可以：打开终端，进入 Tomcat\bin 目录，然后执行：chmod 777 *.sh
    如果启动Tomcat之后，控制台乱码了，那你可以尝试下在 Tomcat VM 参数上加上：-Dfile.encoding=UTF-8
    Git 的路径配置：Preferences -- Version Control -- Git -- Path to Git executable 的值是：/usr/local/git/bin/git
    如果已经安装了zsh，终端路径可以改成zsh的，配置方法在 Preferences -- Tools -- Terminal -- Shell path 的值改为是：/bin/zsh
    ```
- IDEA在Mac下的配置文件保存路径
    ```
    下面内容中：XXXXXX，表示 IntelliJ IDEA 的版本号，IntelliJ IDEA 的配置目录是跟版本号有关系的。
    /Users/你的用户名/Library/Application Support/IntelliJIdeaXXXXXX，用于保存安装的插件
    /Users/你的用户名/Library/Caches/IntelliJIdeaXXXXXX，用于保存缓存、日志、以及本地的版本控制信息（local history 这个功能）
    /Users/你的用户名/Library/Preferences/IntelliJIdeaXXXXXX，用于保存你的个人配置、授权文件，等价于 Windows 下的 config 目录
    ```

### **Maven**

- 官网下载：<http://maven.apache.org/download.cgi>
- Maven是绿色版的，任何系统都适用。
- 安装方式和Windows、Linux没啥本质区别，都是把zip文件夹解压，然后新增几个系统变量，修改Maven配置文件参数。
    ```
    把Maven解压后，直接把Windows的settings.xml复制过来，修改下该文件本地仓库的路径，其他没啥可以改的了。
    然后本地仓库的那些依赖包是直接从Windows下拷贝过来的，这个是任何系统下都兼容的，不需要额外处理。
    最后再用DEA对Maven 配置路径重新做下修改。
    ```
    ```
    # Maven
    export MAVEN_HOME=/Users/你的用户名/my_software/work_software/maven3.3.9
    export PATH=$MAVEN_HOME/bin:$PATH
    ```

### **Gradle**

- 官网下载：<https://gradle.org/releases>
- Gradle和Maven思路是一模一样的。
    ```
    # Gradle
    GRADLE_HOME=/Users/youmeek/my_software/work_software/gradle4.2
    PATH=$PATH:$GRADLE_HOME/bin
    export GRADLE_HOME
    export PATH
    ```

### **Docker**

- 官网下载：<https://download.docker.com/mac/stable/Docker.dmg>
- 修改register的方式可以参看这篇文章：<http://www.runoob.com/docker/macos-docker-install.html>

### **MySQL5.7**

- 官网下载：<http://dev.mysql.com/downloads/mysql>
- MySQL5.7版本：<https://dev.mysql.com/downloads/mysql/5.7.html#downloads>
- MySQL官网提供的Mac系统的安装包，是下一步下一步安装类型的，安装结束后，会提示你它生成的一个随机密码，你要复制下，等下要用。
- 如何修改root密码
    ```
    打开：系统偏好设置 -- 底部的 MySQL -- 点击：Stop MySQL Server，根据提示输入你的 Mac 用户密码。
    连接：sudo /usr/local/mysql/bin/mysql -h 127.0.0.1 -u root -P 3306 -p，输入刚刚复制的密码
    – 修改密码：set password = password('123456');
    ```
- MySQL配置文件设置(vim /etc/my.cnf)
    ```
    [mysql]
    default-character-set = utf8mb4
    [mysqld]
    symbolic-links=0
    log-error=/var/log/mysql/error.log
    default-storage-engine = InnoDB
    collation-server = utf8mb4_unicode_520_ci
    init_connect = 'SET NAMES utf8mb4'
    character-set-server = utf8mb4
    lower_case_table_names = 1
    max_allowed_packet = 50M
    ```

### **Node**

- `Homebrew方式(推荐，因为避免权限问题)`：brew install node
    ```
    # 卸载
    brew uninstall node
    sudo rm -rf /usr/local/share/systemtap
    sudo rm -rf /usr/local/lib/dtrace/node
    ```
- 官网安装包下载：<https://nodejs.org/zh-cn>
    ```
    # 卸载
    sudo rm -rf /usr/local/{bin/{node,npm,node-debug,node-gyp},include/node*,lib/node_modules/npm,lib/node,share/man/*/node*}
    sudo rm -rf ~/.npm*
    sudo rm -rf ~/.node*
    sudo rm -rf /usr/local/lib/node*
    sudo rm -rf /usr/local/share/doc/node*
    ```
- 使用nrm切换源
    ```
    安装：npm install -g nrm
    列表源：nrm ls
    使用源：nrm use taobao
    ```
- npm安装很容易出现权限相关的问题，遇到了可以类似这样：sudo npm install --unsafe-perm=true --allow-root --global gulp

## 参考链接

- <http://www.youmeek.com/mac-java>

## 结束语

未完待续...
