---
title: everything极速搜索
date: 2024-02-18 15:18:12
tags:
  - everything
  - 搜索
categories:
  - tools
cover: https://s2.loli.net/2024/06/07/agix2nv7zhCYGJj.webp
---



# everything

## 1.指定文件后缀

搜索文件名，并且==要求后缀==，假设要搜索 名称包含`main`的所有`java`文件,可以搜索

> [!note]
>
> `main ext:java`
>
> ext:java 就是指定后缀名，这个条件和main条件顺序随意，==注意中间要有一个空格==

## 2.指定文件路径

搜索文件，并且要求==该文件包含指定路径==，假设要搜索`base.css`文件，并且该文件路径中包含`typora`

> [!note]
>
> `base.css path:typora`
>
> 使用`path`指定文件包含的路径

## 3.指定文件夹

在**指定文件夹**下搜索文件，假设要在文件夹下`"D:\QQ\"`文件夹下搜索文件`qq.exe`

> `"D:\QQ\" qq.exe`
>
> 前面写文件夹路径，后面接文件名，中间有空格

## 4.文本内容

假如搜索到的文件有很多个，我只想要文件中**内容包含123**的怎么查询?

> 使用`content`指定文件中包含的内容
>
> `1.txt content:123`

更多用法见[voidtools](https://www.voidtools.com/zh-cn/)
