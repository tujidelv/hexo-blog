---
title: 命令行下载神器youtube-dl
date: 2020-03-29 17:28:33
categories:
- 软件
- 资源下载
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

- **[youtube-dl](https://github.com/ytdl-org/youtube-dl)**是一个开源跨平台的命令行下载神器，可以从YouTube.com和其他一些网站中下载视频。
- 支持的网站列表：<https://ytdl-org.github.io/youtube-dl/supportedsites.html>
- 如果需要使用图形化工具界面操作，可以下载**[youtube-dl-gui](https://github.com/MrS0m30n3/youtube-dl-gui)**。
- 与之类似的还有**[you-get](https://github.com/soimort/you-get)**，这个工具对国内的网站支持的比较好一些。
- 如果需要下载的视频网站现在不能用youtube-dl下载的,可以试试You-Get，配合起来用。
---
```
下载油管视频的前提是能够科学上网，这里介绍几种下载视频/音频的思路。
1.浏览器在线下载(简单快捷，上手最快)
    可以通过一些网站提供的在线下载服务进行下载，比如下边这些网站，只需要将youtube视频网址粘贴一下，就可以下载。
    https://en.savefrom.net/1-youtube-video-downloader-1/
    http://www.youtube-video-downloader.xyz/
    https://www.youtuhi.com/
2.浏览器插件(上手稍慢，可以边看边下载)
    方法1,先安装油猴Tampermonkey插件,再去搜索相应的视频下载脚本。
    方法2,直接安装相应的视频下载插件。
3.命令行下载程序(上手难度大，功能最强大)
    这类的也有很多,例如上面提到的youtube-dl、you-get,有些还有相应的图形化工具。
```

## 开始

### `安装`

- 类Unix用户(Linux,MacOS等)
    ```
    sudo curl -L https://yt-dl.org/downloads/latest/youtube-dl -o /usr/local/bin/youtube-dl
    sudo chmod a+rx /usr/local/bin/youtube-dl
    ```
    ```
    sudo wget https://yt-dl.org/downloads/latest/youtube-dl -O /usr/local/bin/youtube-dl
    sudo chmod a+rx /usr/local/bin/youtube-dl
    ```
- Windows用户
    1. 从**[ffmpeg官网](https://ffmpeg.org)**下载[ffmpeg](https://ffmpeg.zeranoe.com/builds/)，注意选择Windows版本，解压后放到任意目录。
    2. 从**[youtube-dl官网](http://ytdl-org.github.io/youtube-dl)**下载[youtube-dl.exe](https://ytdl-org.github.io/youtube-dl/download.html)，然后放在上一步得到ffmpeg的`bin`目录下。
    3. 将ffmpeg的bin文件夹（D:\ffmpeg\bin）路径加入系统PATH里面。
    4. 测试ffmpeg与youtube-dl，打开命令行，分别输入`ffmpeg -version;youtube-dl --version`，没报错就成功了。
    ---
    ```
    Tips：
    1. 升级ffmpeg，去官网按照系统下载最新版、替换即可。
    2. 升级youtube-dl，只需要在命令行里面运行`youtube-dl --proxy  "http://127.0.0.1:1080"  -U`。
    3. 设置环境变量时不要放入%SYSTEMROOT%\System32（例如，不要放入C:\Windows\System32）。
    ```
- MacOS用户
    ```
    可以使用Homebrew安装youtube-dl：brew install youtube-dl
    ```

### `使用`

```
youtube-dl [OPTIONS] URL [URL...]    程序自动选择一个最清晰的格式下载视频/播放列表
    默认情况下，youtube-dl尝试下载最佳质量，即如果您希望获得最佳质量，则不需要通过任何特殊选项，youtube-dl将默认为您猜测。
```
   
### `选项`

- 查看相关
```
-h, --help                  命令帮助
--version                   查看程序版本
-U, --update                更新程序到最新版本。确保您有足够的权限（如果需要，使用sudo运行）
--list-extractors           列出所有支持的提取器/网站列表
--extractor-descriptions    列出所有支持的提取器的输出描述
--flat-playlist             不提取播放列表中的视频，只列出它们
-F                          查看视频有哪些格式，方便进行针对型的下载
```
- 下载相关
```
-i, --ignore-errors                     当下载错误时继续，例如跳过播放列表中不可用的视频
--cookies /path/to/cookies/file.txt     将Cookie传递给youtube-dl(私人视频需要)
-f, --format [format code/best/worst]   下载指定格式的视频，或者选择带有视频和音频的单个文件代表的最佳质量/质量最差格式。默认：-f bestvideo+bestaudio/best
--download-archive /path/to/file.txt    仅从播放列表下载最新视频
--proxy URL                             Use the specified HTTP/HTTPS/SOCKS proxy.For example socks5://127.0.0.1:1080/.
--skip-download                         不下载视频，例如只下载字幕文件、图片等等
```
```
--write-sub                             下载视频的所有可用字幕中的默认第一个(Available subtitles)
--all-subs                              下载视频的所有可用字幕(Available subtitles)
--write-auto-sub                        下载视频的自动生成的字幕文件中的默认第一个(YouTube only)
--list-subs                             列出视频的所有可用字幕(油管的字幕分为系统自动生成的字幕和手动上传的字幕)
--sub-format FORMAT                     字幕格式，接受格式首选项，例如：“srt”或“ass/srt/best”
--sub-lang LANGS                        要下载的字幕的语言（可选）用逗号分隔，需搭配write-auto-sub一起使用
--write-thumbnail                       将缩略图写入磁盘
--write-all-thumbnails                  将所有缩略图格式写入磁盘
--list-thumbnails                       模拟并列出所有可用的缩略图格式
--write-description                     Write video description to a .description file
--write-info-json                       Write video metadata to a .info.json file
--write-annotations                     Write video annotations to a .annotations.xml file
```
```
--date/datebefore/dateafter DATE        Download only videos uploaded in this date or before or after
    绝对日期：日期格式YYYYMMDD。
    相对日期：日期格式 (now|today)[+-][0-9](day|week|month|year)(s)?
    $ youtube-dl --dateafter now-6months   //仅下载最近6个月内上传的视频 
    $ youtube-dl --date 19700101  //仅下载1970年1月1日上载的视频 
    $ youtube-dl --dateafter 20000101 --datebefore 20091231   仅下载200x十年期间上传的视频 
```
- 输出选项
```
-o, --output TEMPLATE                   Output filename template, see the "OUTPUT TEMPLATE" for all the info
		eg: -o "%(autonumber)05d-%(upload_date)s-%(title)s-%(id)s.%(ext)s"
		--------------
		id （字符串）：视频标识符
		title （字符串）：视频标题
		url （字符串）：视频网址
		ext （字符串）：视频文件扩展名
		alt_title （字符串）：视频的辅助标题
		display_id （字符串）：视频的备用标识符
		uploader （字符串）：视频上传者的全名
		license （字符串）：视频使用许可的许可名称
		creator （字符串）：视频的创建者
		release_date （字符串）：视频发布的日期（YYYYMMDD）
		timestamp （数字）：视频可用的那一刻的UNIX时间戳。
		upload_date （字符串）：视频上传日期（YYYYMMDD）
		uploader_id （字符串）：视频上传者的昵称或ID
		channel （字符串）：上传视频的频道的全名
		channel_id （字符串）：频道的ID
		location （字符串）：录制视频的实际位置
		duration （数字）：视频长度（以秒为单位）
		view_count （数字）：有多少用户在平台上观看了视频
		like_count （数字）：视频的正面评价数
		dislike_count （数字）：视频的负面评价数
		repost_count （数字）：视频的转发次数
		average_rating （数字）：用户给出的平均评分，所使用的比例取决于网页
		comment_count （数字）：视频评论数
		age_limit （数字）：视频的年龄限制（年）
		is_live （布尔值）：此视频是实时流还是固定长度的视频
		start_time （数字）：开始播放的时间（以秒为单位），如URL中所指定
		end_time （数字）：复制应结束的时间（以秒为单位），如URL中所指定
		format （字符串）：格式的易于理解的描述
		format_id （字符串）：指定的格式代码 --format
		format_note （字符串）：有关格式的其他信息
		width （数字）：视频的宽度
		height （数字）：视频的高度
		resolution （字符串）：宽度和高度的文字描述
		tbr （数字）：音频和视频的平均比特率，单位为KBit / s
		abr （数字）：平均音频比特率，单位为KBit / s
		acodec （字符串）：使用中的音频编解码器的名称
		asr （数字）：音频采样率，以赫兹为单位
		vbr （数字）：平均视频比特率，单位为KBit / s
		fps （数字）：帧频
		vcodec （字符串）：正在使用的视频编解码器的名称
		container （字符串）：容器格式的名称
		filesize （数字）：字节数（如果事先知道）
		filesize_approx （数字）：估计的字节数
		protocol （字符串）：将用于实际下载的协议
		extractor （字符串）：提取器的名称
		extractor_key （字符串）：提取器的键名
		epoch （数字）：创建文件时的Unix纪元
		autonumber （数字）：五位数字，每次下载都会增加，从零开始
		playlist （字符串）：包含视频的播放列表的名称或ID
		playlist_index （数字）：播放列表中视频的索引，根据播放列表的总长度用前导零填充
		playlist_id （字符串）：播放列表标识符
		playlist_title （字符串）：播放列表标题
		playlist_uploader （字符串）：播放列表上传者的全名
		playlist_uploader_id （字符串）：播放列表上传者的昵称或ID
```

## 常见问题

1. 经常更新youtube-dl保证功能最新而且稳定
    ```
    根据经验，youtube-dl每月至少发布一次，并且通常每周发布一次，甚至每天发布一次。
    youtube-dl -U
    ```
2. 安装ffmpeg获取高质量视频(安装步骤见参考链接)
    ```
    youtube-dl在大多数网站上都能正常运行，但是如果需要下载更高质量的视频，则需要avconv或ffmpeg。
    当youtube-dl知道某个特定的下载程序对于给定的网站效果更好时，就会选择该下载程序。否则youtube-dl会选择最佳的下载器以实现一般兼容性，而目前恰好是ffmpeg。
    ```
    ```
    youtube默认使用bestvideo+bestaudio/best格式选项,如果安装了ffmpeg或avconv，则将导致分别下载bestvideo和混合bestaudio并将它们混合到一个文件中，
    从而提供最佳的整体质量。否则，它会退回best并导致以单个文件的形式下载最佳质量的文件。
    如果您不希望获得分辨率高于1080p的视频，则可以添加-f bestvideo[height<=?1080]+bestaudio/best到您的配置文件。
    ```
3. 将下载内容放入特定文件夹
    ```
    使用-o来指定输出模板，例如-o "/home/user/videos/%(title)s-%(id)s.%(ext)s"。如果您希望所有下载都使用此选项，请将选项放入配置文件中。
    ```
4. 将Cookie传递给youtube-dl
    ```
    使用--cookies选项，例如--cookies /path/to/cookies/file.txt。
    为了从浏览器中提取Cookie，请使用任何符合要求的浏览器扩展程序来导出Cookie。
    当特定提取程序未明确实现Cookie时，将cookie传递给youtube-dl是解决登录问题的好方法。
    ```
5. 仅从播放列表下载新视频
    ```
    使用下载存档功能。使用--download-archive先下载完整的播放列表，该列表会将所有视频的标识符记录在一个特殊文件中。
    随后的每次相同运行均--download-archive将仅下载新视频，并跳过之前已下载的所有视频。请注意，只有成功的下载记录在文件中。
    ```
6. 检测youtube-dl是否支持给定的URL
    ```
    首先查看支持的站点列表。请注意有时网站可能会更改其网址方案（例如，从https://example.com/video/1234567更改为https://example.com/v/1234567）
    而youtube-dl报告的网址为该列表中的服务不受支持。在这种情况下，只需报告一个issues。
    ```

## 参考链接

<https://github.com/ytdl-org/youtube-dl>
<https://github.com/MrS0m30n3/youtube-dl-gui>
<https://my.oschina.net/ososchina/blog/827182>

## 结束语

- 未完待续...

```
youtube-dl -F -i --proxy "socks5://127.0.0.1:1080" --cookies /opt/setups/cookies.txt --download-archive archive.txt --write-info-json --all-subs --write-all-thumbnails -o "%(upload_date)s_%(title)s_%(id)s.%(ext)s"
```
