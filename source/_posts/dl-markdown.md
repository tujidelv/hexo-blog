---
title: Markdown 札记
date: 2018-12-01 15:54:44
categories:
- 前端
- 开发语言
tags:
- markdown
---

# Markdown 札记

## 目录

- [简介](#简介)
- [常用语法](#常用语法)
- [写作风格](#写作风格)
- [参考链接](#参考链接)
- [结束语](#结束语)

## 简介

- 宗旨：实现 `易读易写`
- 百度百科：
  > Markdown 是一种可以使用普通文本编辑器编写的标记语言，通过简单的标记语法，它可以使普通文本内容具有一定的格式。

  > Markdown 具有一系列衍生版本，用于扩展 Markdown 的功能（如表格、脚注、内嵌HTML等等），这些功能原初的 Markdown 尚不具备，它们能让Markdown 转换成更多的格式，例如 LaTeX，Docbook。Markdown 增强版中比较有名的有 Markdown Extra、MultiMarkdown、Maruku 等。这些衍生版本要么基于工具，如 Pandoc；要么基于网站，如 GitHub 和 Wikipedia，在语法上基本兼容，但在一些语法和渲染效果上有改动。
- 维基百科：
  > Markdown 是一种轻量级标记语言，创始人为约翰·格鲁伯（英语：John Gruber）。它允许人们“使用易读易写的纯文本格式编写文档，然后转换成有效的 XHTML（或者 HTML）文档”。这种语言吸收了很多在电子邮件中已有的纯文本标记的特性。

  > 由于Markdown的轻量化、易读易写特性，并且对于图片，图表、数学式都有支持，目前许多网站都广泛使用 Markdown 来撰写帮助文档或是用于论坛上发表消息。例如：GitHub、reddit、Diaspora、Stack Exchange、OpenStreetMap 、SourceForge 等。甚至 Markdown 能被使用来撰写电子书。

## 常用语法

- ### 标题

MarkDown 支持 1~6 级标题，通过加在标题文字前的 # 来区分。例如，
```
# 这是一级标题
## 这是二级标题
...
###### 这是六级标题
```
Tips：# 和标题文字之间是有一个空格的。

- ### 段落

一行文字就是一个段落。例如，
```
这是一行文字，MarkDown 中的段落。
```
Tips：如果要换行，那么两行之间要隔一个空行或者在第一行末尾敲2个空格或者`<br>`。

- ### 粗体和斜体

Markdown 中字体的粗体和斜体用 * ** 表示。例如，
```
*这里是斜体*
**这里是粗体**
***这里是粗体 + 斜体***
```

- ### 删除线

可能有一部分编辑器不支持这个元素,可用 `<del>` 标签来替代。删除线用 ~ 定义。例如，
```
~~我来了木森~~
```
Tips：删除线要用两个 ~ 来包裹文字。

- ### 引用

在一行文字前添加大于号 > 来使用引用格式。例如，
```
> 这里是引用句子。
```
引用可以嵌套使用，例如，
```
>> 这里使用了嵌套引用。
```
也可以嵌套其他格式。例如，
```
> ## 嵌套一个二级标题。
> *嵌套斜体字。*
> **嵌套粗体字。**
```
Tips：想要结束引用可以空一行。

- ### 列表

列表分为无序列表和有序列表以及任务列表。  
无序列表使用加号、减号和星号来标记，后面需跟 `空格`。例如，

```
+ 加号列表
+ 加号列表
+ 加号列表
- 减号列表
- 减号列表
- 减号列表
* 星号列表
* 星号列表
* 星号列表
```
有序列表使用自然数加上英文句点来标记，后面需跟 `空格`。例如，
```
1. 有序列表
2. 有序列表
3. 有序列表
```
任务列表是标记为[ ]或[x]（未完成或完成）的项目的列表。例如：

```
- [ ] 这是一个任务列表项
- [ ] 需要在前面使用列表的语法
- [ ] normal **formatting**, @mentions, #1234 refs
- [ ] 未完成
- [x] 完成
```

Tips：二级列表只需在一级列表前面使用 `Tab` 键。

- ### 代码块

有两种风格，原生和 Github 。  
原生风格，首行缩进四个空格。例如，

```
    这是一个原生风格代码块。
```
Github 风格，代码块的前后用三个反引号独占一行来标记。例如，
<pre>
```java
这是一个GitHub风格代码块。
```
</pre>

Tips：Github风格可在开头的 ``` 后面跟上语言名称，例如java等。

- ### 标亮(内联代码)

内联代码用反引号 ` 表示。例如，
```
`请把我标亮`
```
Tips：有的编辑器用单个 **\`** 不起作用，只好用两个 **\`** 将代码包裹起来。

- ### 分割线

分割线可以用三个以上的星号、减号或者下划线来标识，行内不能有其他东西，但是可以插入空格。例如，
```
***
* * * 
---
- - - 
___
_ _ _
```

- ### 超链接

链接用下面的格式标识。例如，
```  
[链接要显示的文字](链接URL "鼠标箭头放到链接上的提示文字，可以不写，与链接之间留有空格")
```
还可以用另外的格式标识，效果也是一样的，例如，
``` 
[链接要显示的文字][id]

[id]:链接URL "鼠标箭头放到链接上的提示文字，可以不写，与链接之间留有空格"
```
Tips：如果链接的地址和要显示的文字一样的话，可以用尖括号将其包裹形成自动连接。

- ### 图片

图片的标识语法与链接类似。例如，
```
![图片不存在时的提示文字](图片URL "鼠标箭头放到图片上的提示文字")
```
图片的 Markdown 标识与 HTML 标签有对应的关系，关系如下，
```
![img alt 属性](img 地址 “img title 属性”)
<img src='img 地址' alt='img alt 属性' title='img title 属性' width="16" height="16" align="center">
```
Tips：MD 文件中可采用 `<img>` 标签写法控制图片大小。

- ### 表格

使用 | 来分隔不同的单元格，使用 - 来分隔表头和其他行。  
原本是 Github 风格的语法，大部分编辑器都支持。例如，

```  
| 语言 | 时间 | 备注 |
|----|----:|:------:|
|Java|3年|学不动|
|Python|2年|好学|
|Golang|1年|不好学|
```
Tips:第一行表头可以不要，第二行必须要有，第二行的冒号标识表中内容居左、居右还是剧中，如果不加冒号默认居左。

- ### 内嵌 HTML

Markdown 保留了内嵌 HTML 的语法，用来设置纯 Markdown 不支持的内容，标签中的内容都会输入到结果中。例如，
```
<html>
  <body>
    <div>
        <iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=4877755&auto=1&height=66"></iframe>
      </div>
   </body>
</html>
```
```
<font color="red" size="8">我是红色放大字</font>
```

## 写作风格

- ### 空格
  - 中文与英文之间需要有空格，例如：Apple Watch 将于下周开始接受预订。
  - 中文与数字之间需要有空格，例如：下午跑步 1 小时。
  - 数字与单位之间需要有一个空格，例如：是 5 GB 而不是 5GB。
  - 全角标点符号与其他字符之间不需要空格。例如：第一行表头可以不要，第二行。

- ### 标点符号
  - 只有中文或中英文混排中，一律使用中文/全角标点。
  - 中英文混排中如果出现整句英文，则在这句英文中使用英文/半角标点。
  - 中文标点与其他字符间一律不加空格。
  - 尽量避免重复使用标点（尤其是叠加感叹号、问号等），例如！！！等。

- ### 段落
  - 段落开头不要留出空白字符，顶格写。
  - 段落之间使用一个空行隔开或者在第一行末尾敲2个空格。

- ### 文件命名
  - 文件夹：英文小写（多个英文减号连接）。
    ```
    正确：copywriting-guide
    错误：copywriting_guide、copywritingGuide、CopywritingGuide、copywriting guide
    ```
  - Markdown 文件：英文小写（多个英文减号连接）。
    ```
    正确：copywriting-guide.md
    错误：copywriting_guide.md、copywritingGuide.md、CopywritingGuide.md、copywriting guide.md
    ```
- ### 目录标题
  - 目录标题与每篇文章的标题要一致。

## 参考链接

- [Markdown 语法说明 (简体中文版)](https://www.appinn.com/markdown/)
- [Typora 的 Markdown 语法](https://support.typoraio.cn/zh/Markdown-Reference/)

## 结束语

- 未完待续...

