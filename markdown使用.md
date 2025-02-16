---
title: markdown从入门到上天
date: 2024-03-01 22:13:26
categories: 
    - tools
tags: markdown
cover: https://s2.loli.net/2024/06/09/MgYqOejQcICpFsP.webp
---

# Markdown

## 简介

> [Markdown][markdown] 是一种轻量级的标记语言，可用于在纯文本文档中添加格式化元素。Markdown 由 John Gruber 于 2004 年创建，如今已成为世界上最受欢迎的标记语言之一
>
> 1. 专注于文字内容；
> 2. 纯文本，易读易写，可以方便地纳入版本控制；
> 3. 语法简单，没有什么学习成本，能轻松在码字的同时做出美观大方的排版。

## 基本使用

### 字体

```markdown
*斜体文本*
_斜体文本_
**粗体文本**
__粗体文本__
***粗斜体文本***
___粗斜体文本___
```

*斜体文本*，<kbd>Ctrl I</kbd>(此处只针对`typora`编辑器)
*斜体文本*
**粗体文本**，<kbd>Ctrl B</kbd>
**粗体文本**
***粗斜体文本***
***粗斜体文本***

### 分割线

<kbd>——- enter</kbd>

```markdown
***

* * *

*****

- - -

----------
```

***

* * *

*****

- - -

----------

### 删除线

如果段落上的文字要添加删除线，只需要在文字的两端加上两个波浪线 **~~** 即可，实例如下：~~clb.pages.dev~~

```markdown
~~clb.pages.dev~~
```

### 下划线

<u>下划线</u>可以通过 HTML 的 `<u>` 标签来实现：<u>带下划线文本</u>

```html
<u>带下划线文本</u>
```

### 脚注

脚注是对文本的**补充说明**,鼠标悬浮后会显示内容,脚注一般放在文章最后

```markdown
我会使用[^markdown]

[^markdown]:Markdown 是一种轻量级标记语言，它允许人们使用易读易写的纯文本格式编写文档
```

我会使用[^markdown]

[^markdown]:Markdown 是一种轻量级标记语言，它允许人们使用易读易写的纯文本格式编写文档

### 列表

Markdown 支持有序列表和无序列表

无序列表使用星号(*****)、加号(**+**)或是减号(**-**)作为列表标记，这些标记后面要添加一个空格，然后再填写内容：

```markdown
* 第一项
* 第二项
* 第三项

+ 第一项
+ 第二项
+ 第三项

1. one
2. two
3. three
```

* 第一项
* 第二项
* 第三项

1. one
2. two
3. three

嵌套使用

```markdown
- 1
  - 1.1
    - 1.1.1
      - 1.1.1.1
```

* 1
  * 1.1
    * 1.1.1
      * 1.1.1.1

### 引用

Markdown 引用是在段落开头使用 **>** 符号 ，然后后面紧跟一个**空格**符号：

```markdown
> 引用
> 123
> 456
```

> 引用
> 123
> 456

可嵌套

> 123
>
> > 456

### 代码

如果是段落上的一个函数或片段的代码可以用反引号把它包起来（**\`**），例如：`print()`

```markdown
`print()`
```

你也可以用 **```** 包裹一段代码，并指定一种语言（也可以不指定）：

~~~markdown
```python
print(666)
```
~~~

```python
print(666)
```

------

### 链接

链接使用方法如下：

```
[链接名称](链接地址)

或者

<链接地址>
```

#### 基本使用

[我的主页](https://clb.pages.dev)

```markdown
[我的主页](https://clb.pages.dev)
```

#### 直接使用链接

<https://clb.pages.dev>

```markdown
<https://clb.pages.dev>
```

#### 高级链接

> 如果我的文档中**很多地方都需要使用一个网址**，可以将这个网址**定义为一个变量**放在文章末尾，其他需要使用这个网址的地方直接**通过变量名访问**

```markdown
这是我的[主页][home]

这也是我的[主页][home]

这还是我的[主页][home]

[home]:https://clb.pages.dev
```

这是我的[主页][home]

这也是我的[主页][home]

这还是我的[主页][home]

### 图片

Markdown 图片语法格式如下：

```markdown
![alt 属性文本](图片地址)
![alt 属性文本](图片地址 "鼠标悬浮时的提示文本")

![](https://s2.loli.net/2024/06/02/wuJknzxaFigDSdL.gif)
```

* 开头一个感叹号 !
* 接着一个方括号，里面放上图片的替代文字(可以省略)
* 接着一个普通括号，里面放上图片的网址，最后还可以用引号包住并加上选择性的 '鼠标悬浮时的提示文本'(可以省略)
* 图片的链接也可以定义为变量后使用变量名使用
* **如果要改变图片大小，只能使用`img`标签设定相应属性**

![boqi](https://s2.loli.net/2024/06/02/wuJknzxaFigDSdL.gif)

### 表格

制作表格使用 **|** 来分隔不同的单元格，使用 **-** 来分隔表头和其他行

#### 基本使用

```markdown
|  表头   | 表头  |
|  ----  | ----  |
| 1  | 2|
| 3 | 4|
```

| 表头 | 表头 |
| ---- | ---- |
| 1    | 2    |
| 3    | 4    |

#### 对齐方式

**我们可以设置表格的对齐方式：**

* **-:** 设置内容和标题栏居右对齐。
* **:-** 设置内容和标题栏居左对齐。
* **:-:** 设置内容和标题栏居中对齐。

实例如下：

```
| 左对齐 | 右对齐 | 居中对齐 |
| :-----| ----: | :----: |
| 单元格 | 单元格 | 单元格 |
| 单元格 | 单元格 | 单元格 |
```

> **表格语法比较繁琐，建议使用编辑器插入表格功能**

## 进阶

### HTML 元素

不在 Markdown 涵盖范围之内的标签，都可以直接在文档里面用 HTML 撰写

目前支持的 HTML 元素有：`<kbd> <b> <i> <em> <sup> <sub> <br>`等

在Markdown中，您可以使用多种HTML标签。行内元素，如`<span>`、`<cite>`、`<del>`，可以在Markdown的段落、列表或标题中自由使用。此外，您还可以使用`<a>`和`<img>`等标签来创建链接和图片。

对于块级元素，如`<div>`、`<table>`、`<pre>`和`<p>`，您需要在它们前后添加空行，并且不要使用制表符或空格进行缩进。请注意，在HTML块级标签内部，Markdown语法将不会被处理。例如，`<p>italic and **bold**</p>`将不会显示为斜体和粗体。

**出于安全考虑，并非所有Markdown应用程序都支持在Markdown文档中添加HTML。如果您不确定您的应用程序支持哪些标签，请查看相应的手册或文档**

这里有一个简单的例子，展示了如何在Markdown中混合使用HTML和Markdown语法：

```markdown
这是一个普通段落。

<span style="color: red;">这段文字将显示为红色。</span>

*这是Markdown格式的斜体*

<div>
这是一个HTML块级元素。
</div>

**这是Markdown格式的粗体**
```

以上Markdown代码将产生以下效果：

这是一个普通段落

<span style="color: red;">这段文字将显示为红色。</span>

*这是Markdown格式的斜体*

<div> 这是一个HTML块级元素。 </div>

**这是Markdown格式的粗体**

------

### 任务列表

任务列表使您可以创建带有**复选框**的项目列表。在支持任务列表的Markdown应用程序中，复选框将显示在内容旁边。要创建任务列表，请在任务列表项之前添加破折号`-`和方括号`[ ]`，并在`[ ]`前面加上空格。要选择一个复选框，请在方括号`[x]`之间添加 x

```text
- [x] Write the press release
- [ ] Update the website
- [ ] Contact the media
```

![image-20240927134014360](https://s2.loli.net/2024/09/27/ODtdpIVmSHABYTL.png)

* [x] Write the press release
* [ ] Update the website
* [ ] Contact the media

------

### Emoji 表情

有两种方法可以将表情符号添加到Markdown文件中：将表情符号复制并粘贴到Markdown格式的文本中，或者键入*emoji shortcodes*。

#### 复制和粘贴表情符号

在大多数情况下，您可以简单地从[Emojipedia](https://emojipedia.org/) 等来源复制表情符号并将其粘贴到文档中。许多Markdown应用程序会自动以Markdown格式的文本显示表情符号。从Markdown应用程序导出的HTML和PDF文件应显示表情符号

**Tip:** 如果您使用的是静态网站生成器，请确保将HTML页面编码为UTF-8

#### 使用表情符号简码

一些Markdown应用程序允许您通过键入表情符号短代码来插入表情符号。这些以冒号开头和结尾，并包含表情符号的名称。

```markdown
多一眼看一眼就会:boom:
真好笑！ :joy:
```

![image-20240927134104161](https://s2.loli.net/2024/09/27/OGM4dRCVp7zyb3H.png)

**Note:** 注意：您可以使用此[表情符号简码列表](https://gist.github.com/rxaviers/7360908)，但请记住，表情符号简码因应用程序而异。有关更多信息，请参阅Markdown应用程序的文档

------

### LaTex 数学公式

[参考文章](https://mohu.org/info/symbols/symbols.htm)

行内使用\$\$包围 $a = \log_{10}100$

行间使用\$\$\$\$包围，也可以行内使用
$$
a = \log_{10}100
$$

* 分数: \frac {a} {b} $\frac{a}{b}$
* 根号: \sqrt[3]{x^4} $\sqrt[3]{x^4}$
* 点乘: a \cdot b $a \cdot b$​
* 叉乘: a \times b $a \times b$
* 上标: a^2 b^{1+2} $a^2$  $b^{1+2}$ **如果上标是表达式,则加{},下同**
* 下标: a_i $a_6$ $a_{1+3}$
* 积分: **\int_{a}^{b}** $\int_{a}^{b}$
* 正弦: \sin{} $\sin{x}$
* 余弦: \cos{} $\cos{(y+x)}$
* 对数: **\log_{6}** $\log_{6}10$​
* 大于号：\(\textgreater) $\textgreater$
* 小于号：\(\textless)
* 大于等于：\(\geq) $\geq$
* 小于等于：\(\leq) $\leq$​

### github风格警告框

<img src="https://s2.loli.net/2024/09/27/LIhvkjbP2WFJAQw.png" alt="image-20240927134137634" style="zoom:80%;" />

> [!NOTE]
>
> 提醒 `> [!note]` 该风格警告框适用于github，其他网站不保证兼容性

> [!TIP]
>
> 建议 `> [!tip]`

> [!IMPORTANT]
>
> 重要 `[!important]`

> [!WARNING]
>
> 警告 `[!warning]`

> [!CAUTION]
>
> 注意 `[!caution]`

### IFrame标签

#### 网页

<iframe src="https://ikun-ai.pages.dev" width="200" height="500" scrolling="no"/>

#### 音乐播放器

<audio controls style="width: 800px;padding: 10px;margin: 20px auto;">
    <source src="https://i-120.wwentua.com:446/04262200175320781bb/2024/04/16/090933971d80726c897bc21383fe7ace.mp3?st=DWYBqzF1MlCePRFfzBIRoQ&e=1714142574&b=VUsMQwhbA0NZXgR_aB7AGswXjC7FR5FT3Cb0Mj1TXU7RVvwmBV3lVPVl8BWA_c&fi=175320781&pid=223-167-164-3&up=2&mp=0&co=0" type="audio/mpeg">
    您的浏览器不支持 audio 元素。
</audio>
## typora

### [自定义主题](https://theme.typora.io/doc/zh/Write-Custom-Theme/)

> 在通用设置中打开调试模式

![image-20240927134259142](https://s2.loli.net/2024/09/27/RA8tVjh9JW3Mm47.png)

示例：假设我想修改**引用前面竖杠**的颜色

![recording](https://s2.loli.net/2024/03/23/AgUN2Ve7P1dCaXz.gif)

步骤大概如下：

1. 右键检查元素
2. 使用左上角的选择器选择引用框
3. 在右边样式中找到`boder-left`样式,修改颜色直到满意为止

> **这样修改只是暂时的，如果想要永久生效就要找到对应主题的css文件,找到刚才的属性修改好保存并重启typora(或切换2次主题重新加载当前主题)**

* 样式中信息说明这个属性在`maize.css`文件第`188`行

* 先复制修改好的颜色属性，找到`maize.css`(这是我使用的主题文件,不同主题是不一样的)

* 修改对应属性并保存

* 重启typora

> 你也可以直接将修改好css属性直接粘贴到主题css文件的最后进行覆盖，这样就不用找对应的css在哪了
>
> <span style="color:#ff6b6b;font-weight:bold;">最推荐的办法是在主题文件夹下创建一个文件名为 你使用的主题.user.css文件(例如我使用的是maize,则为maize.user.css)，将要修改的属性直接加入这个文件即可</span>
>
> **这种办法适用于typora中大多数样式**

------

### 图床

> markdown中插入的图片会放到一个文件夹下，如果要将文档发给其他人查看，必须将图片全部一起发过去，并且md文件和图片文件夹要放在一起，路径对不上的话所有图片都会加载不出来，非常的麻烦，一般md中插入图片使用图片的url地址，只要有网就能加载

可以使用一些免费图床(有风险),或者使用`github`搭建免费图床(**不推荐**),最好是使用付费图床(稳定)，可自行上网搜索怎么使用

### 自定义快捷键

打开高级设置

打开`conf.user.json`文件，添加快捷键，快捷键名称必须是在菜单栏中存在的，这是我设置的一些快捷键

![image-20240323141230241](https://s2.loli.net/2024/04/01/dkI7sJr6v1nXw3j.png)

------

[markdown]:https://markdown.com.cn/

[home]:https://clb.pages.dev "我的博客主页"
