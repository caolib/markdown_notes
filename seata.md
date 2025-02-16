---
title: seata
tags:
  - seata
  - 分布式事务
date: 2025-01-11 21:03:44
categories:
  - springcloud
cover: https://seata.apache.org/zh-cn/img/seata_logo.png
---

# Seata

## 1.简介

[Seata][seata]是什么？

> Seata 是一款开源的分布式事务解决方案，致力于在微服务架构下提供高性能和简单易用的分布式事务服务。在 Seata 开源之前，其内部版本在阿里系内部一直扮演着应用架构层数据一致性的中间件角色，帮助经济体平稳的度过历年的双11，对上层业务进行了有力的技术支撑。经过多年沉淀与积累，其商业化产品先后在阿里云、金融云上售卖。2019.1 为了打造更加完善的技术生态和普惠技术成果，Seata 正式宣布对外开源，未来 Seata 将以社区共建的形式帮助用户快速落地分布式事务解决方案。

## 2.快速开始

### 1.示例

在这个架构图中，`Business`模块调用了`Storage`模块和`Order`模块，而`Order`模块又调用了`Account`模块，不同于单体架构，由于各个模块以微服务的方式相互独立，我们不能保证`Business`执行的方法具有事务性，也不能简单的通过`@Transactional`注解实现事务，因为无法确定远程调用是否成功

![](https://seata.apache.org/zh-cn/assets/images/architecture-6bdb120b83710010167e8b75448505ec.png)

### 2.解决方案

> 我们只需要使用一个 `@GlobalTransactional` 注解在业务方法上:

```java
@GlobalTransactional
public void purchase(String userId, String commodityCode, int orderCount) {
    ......
}
```

![](https://seata.apache.org/zh-cn/assets/images/solution-1bdadb80e54074aa3088372c17f0244b.png)









[seata]:https://seata.apache.org/
