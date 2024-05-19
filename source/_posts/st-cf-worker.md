---
title: 通过Cloudflare Worker搭建永久免费节点
date: 2024-05-18 18:05:41
categories:
  - 软件
  - 科学上网
tags:
  - fq
---

## 目录

- [准备工作](#准备工作)
- [edgetunnel](#edgetunnel)
- [epeius](#epeius)
- [参考链接](#参考链接)
- [结束语](#结束语)

## 准备工作

> 何为 Cloudflare Worker ?
> Cloudflare Worker 是 Cloudflare 提供的一种服务，它允许开发者在全球分布的边缘服务器上运行自定义的 JavaScript 代码。 Cloudflare Worker 可以用来处理 HTTP 请求，从而允许开发者通过编写 JavaScript 代码来实现各种功能，例如路由请求、修改请求和响应、执行身份验证、实现缓存策略等。

### `注册Cloudflare账号`

1. 推荐使用新注册的账号来部署服务，打开注册地址：[点击注册](https://dash.cloudflare.com/sign-up)。
2. 输入邮箱地址和密码，点击注册。
3. 添加网站或应用程序，然后点击"返回到 XXX's account"，点击上方的"请验证您的电子邮件地址"，去验证电子邮件，注册成功。

### `注册域名`

> 虽说可以不使用域名，但是推荐大家还是购买自己的域名，毕竟不贵，一年才10元RMB。
> 推荐在Namesilo进行购买，因为他的WHOIS隐私是免费的，可以适当的进行一下隐私保护，而且域名还都挺便宜的。

1. 购买地址：[点击访问](https://www.namesilo.com/) （1.49刀/年起，选择.buzz|.sbs|.cfd会更便宜）。
2. 点击网页右上角的头像注册一个会员，输入用户名、邮箱地址、密码，点击"Create Account"，然后开始注册我们的域名。
3. 输入"lvzhiqiang.icu"，加入购物车，点击"Checkout"结账，会弹出一个窗口默认即可，继续"Checkout"，支付方式根据自己的需求进行选择，然后点击"PAY"。
4. 支付成功跳转到[Dashboard](https://www.namesilo.com/account/)，点击"Active Domains"下面的"Manage"，填写个人资料保存，点击左边的"Domain Manager"就能看到我们注册的域名。
5. 进入域名子页面，点击"Domain will be suspended soon due to the lack of email verification"验证邮箱。
6. 注意域名的到期时间及时续费。

### `托管域名到Cloudflare`

> 将上面注册的域名托管到CF，也就是让它解析在CF中完成，这样的话解析的速度也会更快一些，也为了支持我们等一下的自定义域。

1. 点击CF中的"开始使用"，输入刚注册的域名-》Free-》开始快速扫描-》继续，将CF分配的2组名称服务器填写到域名注册商当中去。
2. 去到域名注册商的域名子页面，编辑NameServer，更新成上一步骤的2组名称服务器，点击SUBMIT，可以在"Domain Manager"列表中"NameServers"看到CF分配的2组名称服务器。
3. 等待激活之后，之后这个域名的所有解析操作都可以在CF中完成。
4. 点击"DNS"，将其中2条系统自带的记录删掉。

## edgetunnel

### `项目介绍`

- 免费的节点，解锁ChatGPT以及全解锁Netflix，最快的时候电信千兆网络跑youtube一度给干到了26w+。
- 早在2020年的双12，zizifn大佬在github创立了edgetunnel项目，翻译过来就是边缘隧道，原理就是使用cloudflare worker的部署方式可以实现vless节点的免费搭建，也就是大家所说的薅羊毛。
- 时隔4年有余，原先的项目已经更新迭代了不少次，也被Fork修改了很多内容，加入了不少的功能，那其中以CM大佬Fork修改的项目最为代表性，以下为搭建原版（初级）和修改版（高级）的步骤。

### `搭建初级VLESS节点`

> 原作者GitHub，源项目地址：[点击访问](https://github.com/zizifn/edgetunnel)

1. 点击Cloudflare左上角图标来到[首页](https://dash.cloudflare.com/)，点击"Workers和Pages"，创建Worker，自定义名称，然后部署！
2. Worker 部署 VLESS。
    - 点击"编辑代码"，清除原先代码，填入如下代码：[点击访问](https://github.com/V2RaySSR/Free-VLESS/)，代码修改完毕以后，点击右边的"部署"-》"保存并部署"，然后点击左边的箭头返回。
    - 因为原项目里面的代码proxyIP为空，所以在访问套了CF等站点会无法打开，这是CF的机制，所以我们需要通过其他一些IP进行转发，所以就有了以下的代码。
    - [点击这里](https://1024tools.com/uuid)，在线生成一个UUID，用于替换下面代码中第七行的UUID，也就是我们VLESS节点的密钥。（或是用V2rayN生成一个）
    - 关于proxyIP，是用于转发CF的一些流量，所以若是存在套了CF的一些网站无法打开或者无法解锁GPT或者奈飞，请更换其中的其他网址，也就是第九行中的部分网址！
3. 设置自定义域。
    - 找到设置-》触发器，添加自定义域，输入`没使用过`的`已经托管在CF上面`的二级域名（xxx.xxx.xxx）。
    - 博主托管在CF上面的域名是`lvzhiqiang.icu`，随便指定一个二级域名的前缀：cf-vless（名字随意），所以就有了二级域名 `cf-vless.lvzhiqiang.icu`。
    - 输入完成，若是输入框变为绿色，证明格式正确，点击添加自定义域。
    - 域证书会显示：正在初始化，等待证书生效，或是直接访问[该域名](cf-vless.lvzhiqiang.icu)，网页显示有内容，证明部署完毕。
4. 访问 https://二级域名/UUID ，即可看到节点信息，里面包含了订阅链接。
    - 至此，节点部署完毕，复制VLESS的订阅链接粘贴到V2rayN里面，解锁了ChatGPT、Netflix ，是不是很 Happy。
    - 以上就是 VLESS Work 原作者的部署策略。

### `节点IP优选`

- 若是速度不理想，可以自行优选CF IP来进行提速。CF优选IP Windows工具：[点击下载](https://github.com/badafans/better-cloudflare-ip)
- 运行IP优选的时候请关闭代理，这样会更准确，记得不要使用TLS优选。
- 在V2rayN中选择节点编辑服务器，该把优选的IP直接填入到地址就可以使用了。

### `搭建高级VLESS节点`

> 高级版的部署其实和初级的部署大同小异，只是多了很多功能，比如自动生成SUB CLASH、SURGE订阅地址、自动优选IP等。
> [CM](https://github.com/cmliu)基于[zizifn](https://github.com/zizifn)的项目进行了二次创作，GitHub地址：[点击访问](https://github.com/cmliu/edgetunnel)（功能还是很多的）

1. 点击Cloudflare左上角图标来到[首页](https://dash.cloudflare.com/)，点击"创建应用程序"-》"Pages"-》"上传资产"。
2. Pages 部署 VLESS。
    - 为项目创建一个名字，点击创建项目。
    - 上传刚才下载下来的worker.zip （从计算机中选择-》上传压缩文件），然后点击"部署站点"-》"继续处理项目"。
    - 回到刚才的项目，找到"设置"-》"环境变量"-》"添加变量"，变量名称：UUID，变量值为刚才生成的UUID，点击保存。
3. 设置自定义域。
    - 找到"自定义域"-》"设置自定义域"，输入`没使用过`的`已经托管在CF上面`的二级域名（xxx.xxx.xxx）。
    - 博主托管在CF上面的域名是`lvzhiqiang.icu`，随便指定一个二级域名的前缀：cf-vless2（名字随意），所以就有了二级域名 `cf-vless2.lvzhiqiang.icu`。
    - 输入完成，点击"激活域"。
4. 重新部署 Pages。
    - 回到项目，找到"部署"，点击下面的"创建新部署"，再次上传刚才的worker.zip文件，保存并部署。
    - 继续处理项目，回到"自定义域"，可以看到`cf-vless2.lvzhiqiang.icu`已经生效，SSL已启用。
    - 回到"部署"-》"访问站点"，若是有内容出现，证明部署成功。
4. 访问 https://二级域名/UUID ，即可看到节点信息，里面包含了订阅链接。
    - 至此，节点部署完毕，我们导入"快速自适应订阅地址"到相对于的客户端软件，进行节点的订阅。
    - 这些订阅地址也就是CM大佬基于原项目增加的一些功能，例如"快速自适应订阅地址"要比我们机场的订阅地址更可靠，它可以自适应我们的一些客户端。
    - 订阅完成会多了很多节点，其实都是一个节点，只是用了不同的IP，原理和刚才初级部署是一样的，只是CM大佬为我们自动进行了IP的优选，还是很方便的。

## epeius

### `项目介绍`

- ca110us大佬了解了CF封禁VLESS代码1101后，担心鸡蛋都放在同一个篮子里，所以基于zizifn大佬的VLESS协议代码开发出来的Trojan协议的代码。
- 这是一个基于 Cloudflare Worker 平台的脚本，在原版的基础上修改了显示 Trojan 配置信息转换为订阅内容。使用该脚本，你可以方便地将 Trojan 配置信息使用在线配置转换到 Clash 或 Singbox 等工具中。

### `免折腾版`

1. 点击Cloudflare左上角图标来到[首页](https://dash.cloudflare.com/)，点击"Workers和Pages"，创建Worker，自定义名称，然后部署！
2. Worker 部署 TROJAN。
    - 点击"编辑代码"，清除原先代码，填入如下代码：[点击访问](https://github.com/cmliu/epeius/blob/main/_worker.js)，代码修改完毕以后，点击右边的"部署"-》"保存并部署"，然后点击左边的箭头返回。
3. 设置自定义域。
    - 找到设置-》触发器，添加自定义域，输入`没使用过`的`已经托管在CF上面`的二级域名（xxx.xxx.xxx）。
    - 博主托管在CF上面的域名是`lvzhiqiang.icu`，随便指定一个二级域名的前缀：cf-trojan（名字随意），所以就有了二级域名 `cf-trojan.lvzhiqiang.icu`。
    - 输入完成，若是输入框变为绿色，证明格式正确，点击添加自定义域。
    - 域证书会显示：正在初始化，等待证书生效，或是直接访问[该域名](cf-trojan.lvzhiqiang.icu)，网页显示有内容，证明部署完毕。
4. 设置密码（一定要修改，不然节点会被别人拿去跑了）。
    - 回到刚才的项目，找到"设置"-》"变量"-》"添加变量"，变量名称：PASSWORD，变量值可以取任意值，点击部署。
5. 设置优选订阅生成器地址。
    - 回到刚才的项目，找到"设置"-》"变量"-》"添加变量"，变量名称：SUB，变量值trojan.fxxk.dedyn.io，点击部署。
6. 访问https://二级域名/password ，即可看到节点信息，里面包含了订阅链接。
    - 至此，节点部署完毕，我们导入"快速自适应订阅地址"到相对于的客户端软件，进行节点的订阅。

### `折腾版`

1. 去掉"SUB"变量，添加"ADD"变量，值为本地优选域名/优选IP(支持多元素之间,或换行作间隔)。
    ```
    bestcf.onecf.eu.org#官方优选域名-Mingyu大佬提供维护
    bestproxy.onecf.eu.org#反代优选域名-Mingyu大佬提供维护
    cfip.xxxxxxxx.tk#官方优选域名-OTC大佬提供维护
    proxy.xxxxxxxx.tk#反代优选域名-OTC大佬提供维护
    acjp2.cloudflarest.link:2053#反代优选域名-KJKKK大佬提供维护
    acsg.cloudflarest.link:2053#反代优选域名-KJKKK大佬提供维护
    acsg3.cloudflarest.link:2053#反代优选域名-KJKKK大佬提供维护
    xn--b6gac.eu.org#↗↘↗.eu.org官方优选域名
    cdn-all.xn--b6gac.eu.org#cdn-all.↗↘↗.eu.org官方优选域名
    cdn-b100.xn--b6gac.eu.org#cdn-b100.↗↘↗.eu.org反代优选域名
    time.cloudflare.com
    shopify.com
    time.is
    icook.hk
    icook.tw
    ip.sb
    japan.com
    malaysia.com
    russia.com
    singapore.com
    skk.moe
    www.visa.com
    www.visa.com.sg
    www.visa.com.hk
    www.visa.com.tw
    www.visa.co.jp
    www.visakorea.com
    www.gco.gov.qa
    www.gov.se
    www.gov.ua
    www.digitalocean.com
    www.csgo.com
    www.shopify.com
    www.whoer.net
    www.whatismyip.com
    www.ipget.net
    www.hugedomains.com
    www.udacity.com
    www.4chan.org
    www.okcupid.com
    www.glassdoor.com
    www.udemy.com
    alejandracaiccedo.com
    nc.gocada.co
    log.bpminecraft.com
    www.boba88slot.com
    gur.gov.ua
    www.zsu.gov.ua
    www.iakeys.com
    www.d-555.com
    fbi.gov
    ```
2. 添加"PROXYIP"变量，值为备选作为访问CloudFlareCDN站点的代理节点(支持多ProxyIP, ProxyIP之间使用,或换行作间隔)。
    ```
    workers.cloudflare.cyou
    my-telegram-is-herocore.onecf.eu.org
    ```
3. 添加"ADDAPI"变量，值为Mingyu大佬的优先IP的API，可自己维护。
    ```
    https://ipdb.api.030101.xyz/?type=bestproxy&country=true
    ```

## 参考链接

<https://v2rayssr.com/worker-vless.html>
<https://github.com/zizifn/edgetunnel>
<https://github.com/cmliu/edgetunnel>
<https://github.com/ca110us/epeius>
<https://github.com/cmliu/epeius>

## 结束语

- 未完待续...
