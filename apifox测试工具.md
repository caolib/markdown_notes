---
title: apifox测试工具
date: 2024-03-18 22:20:23
categories: 测试
tags: 
    - apifox
    - 测试
cover: https://s2.loli.net/2024/06/07/f1vmkl74TSUuIG3.webp
---

# Apifox

[官方文档](https://apifox.com/help/)

## 登录自动更新token

> 问题：使用apifox测试接口时，令牌过期后，需要重新登录，然后复制粘贴替换全局变量的token，有点麻烦，而且不注意可能会复制错误，怎么实现运行登录接口后自动将返回数据中的token值更新，然后其他请求自动携带全新的token呢

### 1.给登录接口添加后置操作

> **1.1 打开项目的登录接口，添加该接口的后置操作**

![1](https://s2.loli.net/2024/03/31/BVyNZqb1Mfm7UsW.png)

> **1.2 随便设置一个变量名字，然后点击小箭头**

![2](https://s2.loli.net/2024/03/31/ZChcWljvr3yNdGx.png)

> **1.3 左边是返回结果的结构，书写jsonpath表达式 ，\$代表左边的json对象，然后就像访问对象属性一样写就可以了，最后看看提取结果和左边是不是对上了，最后点击确定保存，我的token是data中的token，所以是 `$.data.token`**

![img](https://s2.loli.net/2024/03/31/HAPCavXuh9Ns5lY.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

> **1.4 保存接口文档后测试接口**

![img](https://s2.loli.net/2024/03/31/2uJr9ocZqThwCQP.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

> **1.5 如果上面操作无误，点击右上角按钮可以看到在本地环境多了一个变量值token，并且值就是返回结果的token**

![img](https://s2.loli.net/2024/03/31/nW2jkyY3ZHiuz4f.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### 2.设置项目所有接口的auth认证

> **2.1 如图依次点击，key的名字根据自己需要设置，value的值从环境变量读取**

![img](https://s2.loli.net/2024/03/31/HLtYpbIF3w4eSnk.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

> **2.2 读取变量选择之前设置的变量，确定后保存接口的修改**

![img](https://s2.loli.net/2024/03/31/6RGFCTetWnHb3Kh.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

> **3. 测试执行登录接口是否会自动设置token，可以执行登录接口后直接执行其他接口**

![img](https://s2.loli.net/2024/03/31/GWsMZJSzY2Nqw1H.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

每次修改完成后记得保存再运行，这样就能简单完成token的自动更换

## [Apifox IDEA 插件快速上手](https://apifox.com/help/applications-and-plugins/idea/start)

`Apifox Helper` 是 Apifox 团队针对 IntelliJ IDEA 环境所推出的插件，可以在 IDEA 环境中识别本地 Java、Kotlin 后端项目的源代码，自动生成 API 文档并一键同步到 Apifox 的项目中。

对于常见的开发框架，`Apifox Helper` 插件能够做到开箱即用，实现真正的代码零侵入。如下图所示，仅通过识别最基本的 SpringBoot 代码，即可生成一份详尽的 API 文档：

![](https://cdn.apifox.cn/uploads/help/202401101156728.png)
