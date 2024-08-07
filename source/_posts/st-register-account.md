---
title: 主流应用Account注册
date: 2024-04-26 17:32:17
categories:
- 软件
- 科学上网
tags:
- fq
---

## 目录

- [Google账号注册](#Google账号注册)
- [注册美区 AppleID](#注册美区AppleID)
- [参考链接](#参考链接)
- [结束语](#结束语)

## Google账号

### `注册账号`

以下方式均可在无痕模式下进行

- 方法1：链接跳板（免费）
  1. 在谷歌支持页面有一个[创建账号](https://support.google.com/accounts/answer/27441?hl=zh-Hans)的入口，如果你的网络环境足够干净的话，直接点注册账号是有一定的概率是可以绕过这个手机验证直接注册成功的。
  2. 可通过以下方式检测网络环境是否足够干净
     - 检测IP伪装度(分数越高，伪装程度越好，可通过更详细去调整使分数更高)：【[点击进入](https://whoer.net/)】
     - 检测IP地址类型(定位到JSON中company->type，ISP是住宅IP，hosting或者business是机房IP)：【[点击进入](https://ipinfo.io/)】
     - 检查IP欺诈值(风险值越高，这个IP成功率越低)：【[点击进入](https://scamalytics.com/)】
- 方法2：接码平台（付费）
  1. 打开接码平台[sms-activate](https://sms-activate.org/cn)|[5sim](https://5sim.net/)，找到"Google,youtube,Gmail"，选择一些比较偏僻或者稍微贵一些的的国家，能接收到验证码的几率大些；购买后会跳转到接收短信的页面。
  2. 打开谷歌主页，依次点击登录-》创建账号-》个人用途，输入姓名+出生年月+性别+邮箱+密码，进入下一步手机验证，输入上一步选中的手机号，然后填入收到的验证码，填入辅助邮箱，即可完成注册。

### `安全设置`

- 用接码平台手机注册，可能会由于手机号不干净导致有一定的安全隐患，但因为我们绑定了自己的邮箱，如果账号出现被盗或者被封的情况，谷歌会发邮件通知我们的，有一定概率可以通过该邮箱找回账号。
- 账号注册成功之后，我们需要在这个电脑上挂大概一个星期左右，然后将这个账号绑定到自己的手机号上。可通过"安全性-》辅助电话号码-》添加手机号码"完成。
- 绑定手机号后，可通过"安全性-》两步验证"把两步验证打开，这样账号就相对安全了。
- 至于多个邮箱用同一个中国手机号，注册的时候好像不行会提示超限或者收不到验证码，但是注册成功之后换绑或者添加号码是可以无限制并且能收到验证码的。

### `注意事项`

用接码平台注册的话要注意以下几点

1. **账号最好不要作为主账号去使用**。比如说注册谷歌账号的目的是要用到谷歌邮箱，这个邮箱主要用来接收一些其他网站注册的邮件就可以了，但是不要注册一些特别重要的东西例如支付宝、电商平台等涉及到资金管理的一些东西。
2. **如果被封先试着申诉**。账号被封以后，辅助邮箱会收到邮件并附带申诉的链接，主要的理由就是这个账号就是我本人的，没有借过任何人，一直是我自己在使用，大概就是这个意思，然后等消息，是有一定概率找回的。
3. **使用时间越长账号越稳定**。如果必须要作为一个比较重要的账号的话，可以在本地先用上一段时间例如一个月、半年、1年等等，然后再绑定一些比较重要的东西，

## 谷歌云

谷歌云服务器通过其先进的技术基础设施、灵活的服务和强大的安全性功能，为用户提供了一个可靠、高效的云计算平台。并且新用户注册就送300美金，这个羊毛必须要薅！

### `注册`

1. 准备一个自己搭建的节点或者一个好的机场节点、一张外币信用卡（理论上同一张信用卡可多次注册谷歌云账号）、一个提前注册好的谷歌账号（不要用刚注册好的谷歌账号）。
2. 打开无痕模式，打开谷歌官网-》搜索谷歌云-》找到云计算服务-》点击进入；也可直接进入【[谷歌云官网](https://cloud.google.com/?hl=zh-cn)】。
3. 点击免费开始使用-》输入注册好的谷歌邮箱+密码-》建议措施选择以后再说-》国家和地区选择默认识别出来的，最好选择跟谷歌邮箱注册地或者梯子节点地区一致。
4. 进入付款信息验证，账号类型选择个人-》付款方式里输入信用卡卡号，账单邮寄地址可通过[美国地址生成器](https://www.meiguodizhi.com/)生成-》点击免费开始使用，即申请成功。

### `使用`

- 进入[谷歌云控制台](https://console.cloud.google.com/welcome/new)
- 创建防火墙规则。导航栏菜单-》VPC网络-》防火墙，启用Compute Engine API。
    - 入站规则。流量方向：入站；目标：网络中的所有实例；来源IPv4范围：0.0.0.0/0；协议和端口：全部允许；
    - 出站规则。流量方向：出站；目标：网络中的所有实例；目标IPv4范围：0.0.0.0/0；协议和端口：全部允许；
- 创建Google Cloud新实例。导航栏菜单-》Compute Engine-》虚拟机实例。
    - 点击创建实例，左边选择机器的配置，右边可以看到每月估算费用。
    - 区域建议选择离得比较近的亚太地区，这样速度比较快；可用区随便选择就行；启动磁盘根据自己需要选择；访问权限范围选择授予对所有 Cloud API 的完整访问权限；防火墙选择允许 HTTP 流量和允许 HTTPS 流量。
- 修改root密码。
    ```
    # 1、切换到root用户
    sudo -i
    # 2、修改SSH配置文件，找到PermitRootLogin和PasswordAuthentication 默认为no修改为yes
    vi /etc/ssh/sshd_config
    # 3、重启SSH服务
    /etc/init.d/ssh restart 【Debian & Ubuntu】
    /bin/systemctl restart sshd.service【CentOS】
    # 4、设置root账户密码
    passwd root
    ```

### `转移结算账号`

把新的结算账号转移到旧的结算账号上面去，这样就可以一直用旧的结算账号，相对来说会方便一些。

- 新谷歌云账号：导航栏菜单-》结算-》管理结算账号-》重命名结算账号（自定义一个好区分的）-》点击添加主账号-》输入老谷歌云账号（就是老谷歌云账号的邮箱）-》角色选择：Billing Account Administrator-》点击保存。
- 老谷歌云账号：导航栏菜单-》结算-》管理结算账号-》这时可以看到两个结算账号—》回到新谷歌云账号，把新的角色/成员移除，移除后回到老谷歌云账号-》点击我的项目-》找到正在使用的项目，在操作那里点开，点击更改结算信息，选择新的结算账号-》点击设置账号。
- 关闭老谷歌云账号：导航栏菜单-》结算-》管理结算账号-》点击关闭结算账号。

### `私钥连接谷歌云`

解决Finalshell/其它SSH第三方工具连接谷歌云Ubuntu22.04问题，适合Ubuntu22.04及以上的版本，其它的云平台也通用，比如亚马逊云|甲骨文云等。

1. 创建 SSH 密钥
```
ssh-keygen -t rsa -f E:\test\test -C root -b 2048   # -f指定密钥文件路径
```
2. 创建虚拟机
```
登录谷歌云——点击控制台——点击左上角的导航菜单——找到Compute Engine——点击虚拟机实例；
点击创建虚拟机——选择部署的区域——其它保持默认——找到启动磁盘——点击更改——操作系统选择Ubuntu——版本选择22.04或更高——点击选择——勾选允许HTTP流量——勾选HTTPS流量；
点开高级选项——点开安全——点开管理访问权限——找到添加手动生成的SSH密钥——点击添加一项——把前面生成的.pub后缀文件里面的内容粘贴过来——点击创建。
```
3. 下载SSL连接工具Finalshell
```
打开Finalshell——点击蓝色文件夹图标——点击第一个+号——点击SSH连接——名称可以自定义——把服务器IP粘贴到主机位置——方法选择公钥——用户名输入root；
点击浏览——点击导入——名称可以自定义——点击浏览——选择前面创建好的私钥文件（没有后缀那一个）——点击确定——点击确定——点击确定——双击即可连接。
```

## 美区AppleID

### `美区ID的好处`

- 软件下载
  - 美区的苹果应用商店可以下载到其他国家应用商店下载不到的一些APP，例如Google全家桶、TikTok、NetFlix、小火箭、圈 X等等。
- 数据隐私
  - 美区账号的数据是放在美国本土服务器上，而国内账号的数据是放在云上贵州；如果你很注重隐私，美区 ID 是一个不错的选择。

### `准备工作`

1. 干净的邮箱：一个从未注册过 AppleID 的邮箱，最好使用常用的电子邮箱，以免忘记，不要使用临时邮箱，可以使用QQ邮箱、outlook邮箱等。
2. 手机号码：国内外的手机号码都可以，虚拟号码也可，同一个手机号码注册苹果 ID 不要超过 2 次。
3. 浏览器：推荐使用手机浏览器注册，建议开无痕模式，电脑注册，可能会遇到无法注册(此时无法创建你的账户)的情况。

### `开始注册`

1. 首先启用科学上网软件（手机电脑均可），选择美国节点，选择代理/全局模式，使目前网络使用了美国代理。
2. 打开此网站创建您的AppleID：<https://appleid.apple.com/account#!&page=create>。
3. 可以是中文界面，但注意的是国家或地区，必须选择【美国】。按要求填写完整，自己务必记住就行，找回密码或者解锁账号会用到这些问题答案。
    - 美国地址信息生成：<https://www.meiguodizhi.com/>、<https://www.fakepersongenerator.com/>
    - 美国免税州地址：阿拉斯加州（Alaska）特拉华州（Delaware）蒙大拿州（Montana）新罕布什尔州（New Hampshire）俄勒冈州（Oregon）
4. 然后会给你注册邮箱发一个6位数的验证码，输入后就完成了注册。若你是用来共享的账户，登陆后不推荐启用两步验证，这个共享的话会很麻烦，每次都需要进行登陆验证。
---
注：以前注册的账号也可以去该网站修改国家或地区：<https://appleid.apple.com/account/manage>。

## 参考链接

<https://www.youtube.com/watch?v=C1vwxHwRqqo>
<https://github.com/bannedbook/fanqiang/blob/master/ios/AppleID.md>
<https://qiushuizy.com/1659.html>

## 结束语

- 未完待续...