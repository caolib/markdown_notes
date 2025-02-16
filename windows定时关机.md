---
title: windows定时关机
date: 2024-03-19 22:28:39
categories: os
tags: 
    - windows
cover: https://s2.loli.net/2024/06/09/lEWMHmvi2SqV41F.webp
---

1. 打开控制面板，搜索**计划**，点击<mark>计划任务</mark>

![image-20240319223355706](https://s2.loli.net/2024/03/19/8olEi2dyD5xTKcG.png)

2. 右键**任务计划程序库**，创建任务

![image-20240319223747111](https://s2.loli.net/2024/03/19/gpT9YVFaxUEWq3C.png)

3. 常规

![image-20240319223931837](https://s2.loli.net/2024/03/19/Mr2XbwVfNPxUD8g.png)

4. 触发器

![image-20240319224108154](https://s2.loli.net/2024/03/19/oIUu7gqSaji2O83.png)

5. 操作，22:55触发，240秒后启动，也就是22:59关机

![image-20240319224152722](https://s2.loli.net/2024/09/25/upVvQNW6Zt2IdC8.png)

6. 条件

> **添加这个条件后，即使电脑在休眠，也会自动唤醒电脑然后关机！**

![image-20240319224409803](https://s2.loli.net/2024/09/25/Ex9OzeNFTprJBdt.png)

7. 设置![image-20240319224442431](https://s2.loli.net/2024/09/25/6YbUoycx5kMfw7h.png)

最后保存即可，这样就能在断电前一分钟让电脑自动关机了，可以通过查看执行记录看看任务有没有正常执行或者自己写一个脚本添加在关机指令之后的第二条指令
