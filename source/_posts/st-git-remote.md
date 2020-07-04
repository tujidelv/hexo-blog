---
title: Git 札记：远程库
date: 2018-12-01 17:46:49
categories:
- 软件工具
tags:
- 互联网工程
---

# Git 札记：远程库

## 目录

- [简介](#简介)
- [分类](#分类)
- [参考链接](#参考链接)
- [结束语](#结束语)

## 简介

代码托管中心：用来维护远程仓库
```
外网环境：Github,码云等等
内网环境：Gitlab,GitBucket 等等
```

## 外网

### `GitHub`
   
```
- 既是一个免费托管开源代码的远程仓库，可将个人的开源项目放上去；
    还是一个开源协作社区，即可让别人参与你的开源项目，也可参与别人的开源项目。
- Github 常用按钮说明
    - Watch：关注该项目，作者有更新的时候，会在你的 Github 主页有通知消息。
    - Star：收藏该项目，在你的头像上有一个 “Your stars” 链接，可以看到你的收藏列表，以方便下次进来。
    - Fork：复制一份项目到自己的 Github 空间上，你可以自己开发自己的这个地址项目，然后 Pull Request 给项目原主人。
```    
---
- 本地创建 SSH Key

```
1.在用户主目录下，如果有.ssh目录并且该目录下有id_rsa和id_rsa.pub两个文件，则跳过这步；否则在window 下打开Git Bash。
    $ ssh-keygen -t rsa -C "tujide.lv@foxmail.com"
2.一直默认回车，生成的2个文件就是SSH Key的秘钥对，id_rsa是私钥，不能泄露，id_rsa.pub是公钥，可以告诉别人。
```
- GitHub 添加本地 SSH Key

```
1.登陆GitHub，打开"Settings"=>"SSH Keys"页面，点击"New SSH Key"，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容，点击"Add Key"。
    可以添加多个，这样有这些公钥的电脑就可以将本地仓库代码推送到GitHub远程仓库中。
```
- 添加远程仓库

```
1.登录GitHub，打开"New repository",在"Repository name"填入learngit，其他保持默认设置，点击"Create repository"，就创建了一个新的Git仓库。
    该仓库为默认公开的，可通过交保护费让GitHub把仓库变成私有(被微软收购后免费)，或自己搭建Git服务器。
    该仓库默认是空的，可以与本地仓库关联，把本地仓库的内容推送到该仓库。
```
- 关联远程仓库并推送

```
1. 查看远程库的详细信息
    $ git remote -v
2.关联远程库并创建别名
    $ git remote add <远程仓库别名> git@github.com:GitHub账户名/learngit.git
    注：如果在使用命令git remote add时报错(fatal: remote origin already exists.),说明本地库已经关联了一个名叫origin的远程库，有如下2种处理方案。
        1.删除已有的远程仓库origin关联,再重新关联
            - $ git remote rm origin
        2.因为git是分布布,支持同步到多个远程库,远程仓库的名字不能一样
            - $ git remote add gitee git@gitee.com:xxx/xxx.git
            - $ git remote add github git@github.com:xxx/xxx.git
3.将本地库的内容(指定分支)推送到远程库对应的远程分支上(不存在时会自动新建)
    $ git push <远程仓库别名> 分支名
    注：-u 参数，第一次推送时需要,会把本地分支和远程分支关联起来，在以后的推送或拉取可以简化命令。
注：IDEA中可以通过"Share Project on GitHub"将项目直接分享到GitHub上。
```
- 从远程仓库克隆

```
1.SSH协议Clone,没有添加Key将不能推送;Https协议Clone,需要登陆有权限的GitHub帐号才能推送。
    - 没加入团队是无法推送的,项目发起者可在'Settings'->'Collaborators'中邀请GitHub成员加入团队,并把邀请链接发给成员,成员登陆自己的帐号访问邀请链接以同意加入团队,即可进行远程推送。
    - HTTPS协议是无法记住登陆帐号的,第一次推送登陆后,而再次推送无需登陆,是因为win10系统的'凭据管理器'中的'Windows凭据'记住了密码,如果下次想切换别的帐号登陆,可以到此删除掉重新登陆。
2.把远程库下载到本地,因为是读操作,不需要验证身份
    $ git clone git@github.com:GitHub账户名/gitskills.git
    注：会关联远程库并创建别名,同时会把本地仓库master分支与远程仓库的master分支关联起来,在以后的推送或拉取时可以简化命令。
        1.--depth=1,用于指定克隆深度,为1表示只克隆最近一次commit,历史旧数据不clone,可用于解决项目过大时clone导致timeout的问题,但他只会把默认分支 clone 下来,其他远程分支并不在本地,
            如需要的话可如下命令拉取其他分支。
            $ git remote set-branches origin 'remote_branch_name'
            $ git fetch --depth=1 origin remote_branch_name
    注：GitHub还会给出多个地址，如`https://github.com/GitHub账户名/gitskills.git`，因为Git支持多种协议，默认是通过ssh支持的原生git协议，速度最快；也可用https等其他协议，速度慢，
        每次推送必须输入口令。
```
- 拉取

```
1.首先,可以试图用"git push origin branch-name"推送自己的修改。
2.如果推送失败,则因为不是基于GitHub远程库的最新版所做的修改(即远程分支比你的本地更新),需先用"git pull/fetch"抓取远程的新提交,因为是读操作,不需要验证身份。
    - fetch
        - 把远程的更新下载到本地,可"git checkout origin/master"切换查看内容,并没有修改工作区,可"git merge origin/master"进行合并操作。
    - pull = fetch + merge
        - 如果"git pull"提示"no tracking information",则说明没有创建本地分支和远程分支的链接关系,用命令"git branch --set-upstream-to=origin/branch-name branch-name"再进行git pull。
        $ git pull      //如果当前分支只有一个追踪分支,可以省略远程仓库名即可更新并合并
        $ git pull origin      //如果当前分支有多个追踪分支,可以指定当前分支与指定的远程追踪分支进行更新并合并
        $ git pull origin master      //指定当前分支与指定的远程master分支进行更新并合并
        $ git pull origin master1:master2      //指定本地的master2分支与指定的远程master1分支进行更新并合并
3.如果合并有冲突,则手动解决冲突(解决的方法和分支管理中的解决冲突完全一样),并在本地提交。
4.没有冲突或者解决掉冲突后,再用"git push origin branch-name"推送就能成功。
```
- 跨团队协作/参与一个开源项目

```
1.以自己身份进入开源项目主页,点击"fork"克隆到自己的远程仓库下。
2.从自己的远程仓库下克隆项目到本地,这样才能推送修改。ll
3.如果想修复开源项目的一个bug，或者新增一个功能，立刻就可以开始干活，干完后，往自己的远程仓库推送。
4.如果希望开源项目的官方能接受你的修改，可以在GitHub上发起一个pull request,对方是否接受你的pull request就不一定了。
    1. 在自己远程仓库项目主页中,点击'Pull requests'/'New pull request',此时'bask fork'选择官方作者,'head fork'选择自己,点击'Create pull request',填好标题和说明(可选)确认。
    2. 此时官方作者可在项目主页中,点击'Pull requests',能看到自己刚刚发送的请求,进去后可在里面跟请求者对话,点击'Files changed'审核代码,没问题的话可点击'Merge pull request'合并代码。
    3. 在自己远程仓库项目主页中,如果官方作者有回复内容,可在刚发送的请送中看到官方的回复内容,也可回复。
5.如果官方作者有提交新的代码,可在自己项目主页中,点击'Pull requests'/'New pull request',此时'bask fork'选择自己,'head fork'选择官方作者,点击'Create pull request',填好标题和说明(可选)确认。								
    - 没问题的点击'Merge pull request'合并代码以获取官方作者的最新代码。
```
      
### `码云`

- 同 GitHub 一样,提供免费的 Git 仓库;此外，还集成了代码质量检测、项目演示等功能。对于团队协作开发，码云还提供了项目管理、代码托管、文档管理的服务，5 人以下小团队免费。
    - 码云的免费版本也提供私有库功能，只是有 5人 的成员上限。
- 使用方法同 GitHub。

## 内网

### `GitLab`

- 官网地址			
	- 首页：<https://about.gitlab.com/> 
	- 安装说明：<https://about.gitlab.com/installation/>
- 安装命令摘录
    ```shell
    sudo yum install -y curl policycoreutils-python openssh-server cronie
    sudo lokkit -s http -s ssh
    sudo yum install postfix
    sudo service postfix start
    sudo chkconfig postfix on
    curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh | sudo bash
    sudo EXTERNAL_URL="http://gitlab.example.com" yum -y install gitlab-ee
    ```
	 - 实际问题：yum 安装 gitlab-ee(或ce)时，需要联网下载几百 M 的安装文件，非常耗时，所以应提前把所需 RPM 包下载并安装好。
	 下载地址为：<https://packages.gitlab.com/gitlab/gitlab-ce/packages/el/7/gitlab-ce-10.8.2-ce.0.el7.x86_64.rpm>，调整后的安装过程：			
	```shell
	sudo rpm -ivh /opt/gitlab-ce-10.8.2-ce.0.el7.x86_64.rpm		
	sudo yum install -y curl policycoreutils-python openssh-server cronie  		
	sudo lokkit -s http -s ssh		
	sudo yum install postfix		
	sudo service postfix start		
	sudo chkconfig postfix on		
	curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash		
	sudo EXTERNAL_URL="http://gitlab.example.com" yum -y install gitlab-ce
	```
	- 当前步骤完成后重启		
- GitLab 服务操作
	- 初始化配置 gitlab
	    ```shell
	    $ gitlab-ctl reconfigure
		```
	- 启动/停止 gitlab 服务
        ```shell	
        $ gitlab-ctl start/stop
        ```
- 浏览器访问
    - 访问 Linux 服务器 IP 地址即可，如果想访问 EXTERNAL_URL 指定的域名还需要配置域名服务器或本地 hosts 文件。
		- 应该会需要停止防火墙服务
        ```shell
        $ service firewalld stop
        ```
	- 初次登录时需要为 gitlab 的 root 用户设置密码。

### `原生搭建`

1. 安装 Git												
    ```
    $ apt-get install git  (Debian系列系统)
    $ yum install -y git   (RetHat系列系统)
	```
2. 创建Git用户，用来管理 Git服务，并为 Git 用户设置密码
    ```shell
    $ id git
    $ useradd git
    $ echo 'git' | passwd --stdin git &> /dev/null
	```
3. 创建 Git 仓库
    1. 先选定一个目录作为 Git 仓库，假定是/www/wwwroot/hexo.lvzhiqiang.top/hexo-blog.git/，在/www/wwwroot/hexo.lvzhiqiang.top/目录下输入如下命令,Git 
    就会创建一个裸仓库，裸仓库没有工作区。
        ```shell
        $ git init --bare sample.git
        ```
    2. 因为服务器上的 Git 仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的 Git 仓库通常都以 .git 结尾。然后把 owner 改为 git。
        ```shell
        $ chown -R git:git sample.git
        ```
        注：如果执行了第4步,可再执行这个命令。
4. 配置 git hooks 来生成对应的工作区(**非必须**)
    - 这里要使用的是 `post-receive` 的 hook，这个 hook 会在整个 git 操作过程完结以后被运行。
    1. 在/home/data/git/sample.git/hooks目录下创建hook钩子函数，可新建`post-receive`文件,填入如下内容
        ```shell
        #!/bin/sh
        git --work-tree=/www/wwwroot/hexo.lvzhiqiang.top/hexo-blog --git-dir=/www/wwwroot/hexo.lvzhiqiang.top/hexo-blog.git checkout -f
        ```
    2. 为文件添加可执行权限
        ```shell
        $ chmod +x post-receive
        ```
5. Git 打开 RSA 认证
	1. 进入/etc/ssh目录，编辑 sshd_config，打开以下三个配置的注释，然后重启 sshd 服务
        ```shell
        $ vim sshd_config
            RSAAuthentication yes
            PubkeyAuthentication yes
            AuthorizedKeysFile .ssh/authorized_keys
        $ systemctl restart sshd
            或者$ /etc/rc.d/init.d/sshd restart
        ```
        Tips：由 AuthorizedKeysFile 得知公钥的存放路径是.ssh/authorized_keys，实际上是 $Home/.ssh/authorized_keys，由于管理 Git 服务的用户是git，所以实际存放公钥的路径是/home/git/.ssh/authorized_keys
	2. 在/home/git/下创建目录 .ssh,然后把 .ssh 文件夹的 owner 修改为 git
	    ```shell
	    $ mkdir .ssh
	    $ chown -R git:git .ssh
	    ```
6. 创建证书登录(添加客户端公钥到服务器)
	1. 收集所有需要登录的用户的公钥，就是他们自己的 id_rsa.pub 文件，把所有公钥导入到/home/git/.ssh/authorized_keys 文件里，一行一个
	    ```shell
	    $ ssh git@server 'cat >> .ssh/authorized_keys' < ~/.ssh/id_rsa.pub
	    ``` 
	    或者
	    ```shell
	    $ ssh-copy-id -i ~/.ssh/id_rsa.pub git@服务器IP -p 端口号(默认22时可不用此参数)
	    ```
	    Tips：需要输入服务器端 git 用户的密码
	2. 查看服务器端 .ssh 下是否存在 authorized_keys 文件
        ```shell
        $ ll git/.ssh/
	    ```
	3. 修改 .ssh 目录的权限为 700(rwx------)，修改 .ssh/authorized_keys 文件的权限为600(rw-------)
	    ```shell
	    $ chmod 700 .ssh
	    $ chmod 600 authorized_keys
		```
    4. 测试能否登录，此时的 ssh 登录 git 用户不需要密码！否则就有错,可尝试重启`sshd`服务
        ```shell
        $ ssh git@server_ip -p 端口号(默认22时可不用此参数)
        ```
7. 禁止 git 用户 ssh 登录服务器
	- 出于安全考虑，第二步创建的 git 用户不允许登录 shell，这可以通过编辑/etc/passwd 文件完成。将 bash 修改为 git-shell,找到类似下面的一行：
	    ```
	    git:x:1001:1001:,,,:/home/git:/bin/bash改为：git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
	    ```
	- 这样，git 用户可以正常通过 ssh 使用 git，但无法登录 shell，因为我们为 git 用户指定的 git-shell 每次一登录就自动退出。
8. 客户端克隆远程仓库
    ```shell
    $ git clone git@server:/www/wwwroot/hexo.lvzhiqiang.top/hexo-blog.git
    ```
	Tips：如果 SSH 用的不是默认的 22 端口，则需要使用以下的命令（假设 SSH 端口号是 7700）
	```shell
	$ git clone ssh://git@server:7700/www/wwwroot/hexo.lvzhiqiang.top/hexo-blog.git
	```
9. 管理公钥
	- 如果团队很小，把每个人的公钥收集起来放到服务器的/home/git/.ssh/authorized_keys文件里就是可行的。如果团队有几百号人，就没法这么玩了，这时，可以用 Gitosis 来管理公钥。
10. 管理权限
	- 因为 Git 是为 Linux 源代码托管而开发的，所以 Git 也继承了开源社区的精神，不支持权限控制。
	- 不过，因为 Git 支持钩子（hook），所以，可以在服务器端编写一系列脚本来控制提交等操作，达到权限控制的目的，Gitolite 就是这个工具。

### `GitBucket`

- 待更新...

### `Gogs`

- 官⽹：<https://gogs.io>
- 介绍：Gogs是⼀款开源的轻量级Git web服务，其特点是简单易用、文档齐全、国际化做的相当不错。
    ```
    其主要功能如下: 
    1. 提供Http与ssh两种协议访问源码服务。
    2. 提供可WEB界⾯可查看修改源码代码 。
    3. 提供较完善的权限管理功能，其中包括组织、团队、个⼈等仓库权限 。
    4. 提供简单的项⽬viki功能。 
    5. 提供⼯单管理与⾥程碑管理。
    ```
- 安装
    1. 下载 [Gogs](https://gogs.io/docs/installation)，选择Linux amd64
        ```
        $ wget -P /opt/setups/ https://dl.gogs.io/0.11.91/gogs_0.11.91_linux_amd64.zip
        ``` 
    2. 解压到指定目录
        ```
        # mkdir -pv /usr/program
        # unzip gogs_0.11.91_linux_amd64.zip -d /usr/program/
        ```
    3. 启动
        ```
        $ nohup ./gogs web > log.out &
        ```
    4. 访问http://0.0.0.0:3000
- 定时备份与恢复
    - 备份与恢复
        ```bash
        # 查看备份相关参数
        ./gogs backup -h
        # 默认备份,备份在当前目录
        ./gogs backup
        # 参数化备份  --target 输出目录 --database-only 只备份db
        ./gogs backup --target=./backupes --database-only --exclude-repos
        # 恢复 执行该命令前要先删除custom.bak
        ./gogs restore --from=gogs-backup-20180411062712.zip
        ```  
    - ⾃动备份脚本
        ```bash
        #!/bin/sh -e
        gogs_home="/usr/program/gogs"
        backup_dir="$gogs_home/backups"
        
        cd `dirname $0`
        # 执行备份命令
        ./gogs backup --target=$backup_dir
        
        echo 'backup sucess'
        day=7
        #查找并删除 7天前的备份  
        find $backup_dir -name '*.zip' -mtime +7 -type f |xargs rm -f;
        echo 'delete expire back data!'
        ```
    - 添加定时任务
        ```
        # 打开任务编辑器
        crontab -e
        # 输入如下命令 00 04 * * * 每天凌晨4点执行 do-backup.sh 并输出日志至 #backup.log
        00 04 * * * /usr/program/gogs/do-backup.sh >> /usr/program/gogs/log/backup.log 2>&1
        ```

## 参考链接

- [Git 官方文档](https://git-scm.com/book/zh/v2)
- [Git教程 - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

## 结束语

- 未完待续...
- update by 20190801
    ```
    1.如果在github pushr的过程中提示"Connection reset by 13.250.177.223 port 22 fatal: Could not read from remote repository."
    2.可尝试"ping github.com"看是否能Ping通,超时则应该是本地DNS无法解析导致的
    3.可修改"C:\Windows\System32\drivers\etc\hosts"添加
        192.30.255.112  github.com git 
        185.31.16.184 github.global.ssl.fastly.net
    4.可继续2步骤测试
    ```