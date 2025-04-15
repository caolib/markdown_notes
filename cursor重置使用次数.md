---
title: cursor重置使用次数
tags:
  - cursor
date: 2025-03-15 11:13:48
categories: tools
cover: https://www.cursor.com/_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fgradient-hero-prerender.3af0e196.webp&w=3840&q=75
---

# Cursor重置

cursor有一定的免费使用额度，在官网但肯定是不够我们~~白嫖~~使用的，github上有个项目[^go-cursor-help]可以重置账号的使用次数（~~可以无限白嫖了😋~~，接下来讲讲怎么操作

## 1.删除账号

先进入[^cursor]官网，点击右上角进入登录，然后进入账号设置，这里可以看到已经使用的次数（我已经重置过一次了

点击**Delete Account**删除账号

![image-20250315112903872](https://s2.loli.net/2025/03/15/UQZxu8t6MTFYCsj.png)

## 2.重置cursor

来到项目的[^说明文档]复制对应平台的命令，我这里使用windows

```powershell
irm https://aizaozao.com/accelerate.php/https://raw.githubusercontent.com/yuaotian/go-cursor-help/refs/heads/master/scripts/run/cursor_win_id_modifier.ps1 | iex
```

![image-20250315113332101](https://s2.loli.net/2025/03/15/ONpXb3DqmVyk8ih.png)

打开一个powershell（管理员身份）控制台，粘贴运行就ok了

## 3.重新登录

重置完成后，重启cursor后重新注册登录账号就可以了





[^go-cursor-help]:https://github.com/yuaotian/go-cursor-help
[^cursor]:https://www.cursor.com/cn
[^说明文档]:https://github.com/yuaotian/go-cursor-help?tab=readme-ov-file#-one-click-solution
