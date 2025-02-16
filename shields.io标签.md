---
title: shields.io标签生成
date: 2024-03-08 14:17:48
tags:
  - shields.io
  - 标签
categories: tools
cover: https://s2.loli.net/2024/12/27/jbADFXHUvPEy8dV.png
---

![image-20241227135852435](https://s2.loli.net/2024/12/27/jbADFXHUvPEy8dV.png)

> github仓库页面的这种标签是怎么生成的？

## 1.基本使用

[Static Badge | Shields.io](https://shields.io/badges) 可以生成各种标签，可以自定义，比如我想生成一个<mark>背景颜色</mark>蓝色标签，名称为`mytag`就可以使用`mytag-blue`,前面是名称后面是颜色(可以是16进制颜色)![mytag](https://img.shields.io/badge/mytag-blue?style=flat)

![image-20240308142750757](https://s2.loli.net/2024/09/27/i1IeuUvKW4nFLHC.png)

## 2.版本号

也可以指定版本号`mytag-v1.2.3-blue`，注意版本号在颜色前面![version](https://img.shields.io/badge/mytag-v1.2.3-blue?style=flat)

## 3.图标

[Simple Icons](https://simpleicons.org/) 提供了很多产品的logo，比如我想使用redis的logo，直接搜索redis，点击右下角可以复制名字，左下角可以复制配色

<img src="https://s2.loli.net/2024/09/27/YuUsmk7tqvjQnfN.png" alt="image-20240308143650593"  />

返回sheilds页面，点击展开显示更多参数

![image-20240308144647113](https://s2.loli.net/2024/09/27/winJ5Dkdu2Hj38C.png)

生成后的效果 ![Static Badge](https://img.shields.io/badge/Redis-v7.0.12-%23DC382D?style=flat&logo=redis&logoColor=%23DC382D)

## 4.动态生成标签

以仓库提交数为例，搜索commit,注意==cacheSeconds==的设置

<img src="https://s2.loli.net/2024/09/27/oKbY3Hk5aQ2OwWD.png" alt="image-20240309173715898" style="zoom:80%;" />

![GitHub commit activity](https://img.shields.io/github/commit-activity/t/tankingcao/java_design?style=flat&labelColor=red&cacheSeconds=3600)

|     标签     |    搜索关键字    |                             示例                             |
| :----------: | :--------------: | :----------------------------------------------------------: |
|  仓库提交数  |     `commit`     | ![GitHub commit activity](https://img.shields.io/github/commit-activity/t/tankingcao/java_design?style=flat&labelColor=red&cacheSeconds=3600) |
| 仓库发行版本 | `github release` | ![GitHub Release](https://img.shields.io/github/v/release/tankingcao/java_design?include_prereleases&sort=date&display_name=release&style=flat&cacheSeconds=3600) |
