---
title: sentinel
tags:
  - sentinel
date: 2025-01-09 15:19:04
categories:
  - springcloud
  - 服务保护
cover: https://sentinelguard.io/img/sentinel.png
---

# [sentinel][sentinel]

## 1.简介

> **Sentinel** 是面向分布式、多语言异构化服务架构的**流量治理组件**，主要以流量为切入点，从**流量控制**、流量路由、**熔断降级**、系统自适应**保护**等多个维度来帮助用户保障微服务的稳定性。

## 2.快速使用

### 1.下载

前往[sentinel][sentinel-release]的发布页面下载sentinel的jar包，最好放到一个**没有中文目录的文件夹**

### 2.启动

```shell
java -jar .\sentinel-dashboard-1.8.8.jar
```

### 3.额外配置

> 可以通过命令行传递参数对sentinel进行一些额外的配置，但是每次都需要使用一长串命令很不方便，还有一个更好的办法，这个办法对springboot应用的jar包都有用

在jar包的同一个文件夹下新建一个 `application.yml` 文件，然后在文件中配置想要配置的参数，然后使用java命令启动时**会自动读取这个文件的配置，而且优先级更高**，下面是一个示例

```yml
# 端口
server:
    port: 9999 
# sentinel控制台的地址
csp:
    sentinel:
        dashboard:
            server: localhost:9999
# 项目名称
project:
    name: sentinel
# 允许ANSI颜色输出，打开这项后控制台输出的日志带有颜色，但是如果控制台不支持显示颜色的话会乱码，按需打开
spring:
    output:
        ansi:
            enabled: always

```

另外可以在命令中指定**日志文件输出位置**，因为日志默认是输出到C盘的，`-Dcsp.sentinel.log.dir=X:/env/sentinel/logs`指定了日志输出的文件夹，可以自行修改

```shell
java  -Dcsp.sentinel.log.dir=X:/env/sentinel/logs -jar sentinel-dashboard-1.8.8.jar
```

这样的话命令会显得很长，可以将这条命令保存为 `sentinel.cmd` 文件，双击执行即可

> 如果你使用 `pwsh` 的话还有更方便的办法
>
> 1. 找到powershell的配置文件，在控制台使用`echo $PROFILE`打印配置文件路径
>
> 2. 在配置文件中添加配置
>
>    ```sh
>    function sentinel {
>       cd X:\env\sentinel
>       ./sentinel.cmd
>    }
>    ```
>
> 3. 打开一个终端，输入`sentinel`即可启动，既方便又优雅
>
>    ![image-20250109182558016](https://s2.loli.net/2025/01/09/L7yEr2NhgbqMnml.png)

### 4.控制面板

sentinel启动后可以打开[localhost:9999](http://localhost:9999)查看，用户名和密码默认都是`sentinel` 

![image-20250109183123528](https://s2.loli.net/2025/01/09/e4q8EuMsPBzbioK.png)

### 5.引入项目

#### 1.添加依赖

```xml
<!--sentinel-->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
</dependency>
```

#### 2.添加配置

```yml
spring:
  cloud:
    sentinel:
      transport:
        # 控制台地址
        dashboard: localhost:9999
      # 通过请求方法区分
      http-method-specify: true
```

#### 3.访问接口

访问项目的接口，然后回到sentinel控制台查看，可以看到对应接口的访问信息等

![image-20250109192951159](https://s2.loli.net/2025/01/09/PTQEkGODmz7rsHM.png)

## 3.流量控制

#### 1.QPS

> QPS是Queries Per Second的缩写，意思是每秒查询率或每秒请求数。

sentinel中的簇点链路对应controller层中的每个接口，配置流控规则可以对这些接口的请求进行限流，下图对用户登录接口进行限流，如果每秒请求数超过10个就会返回错误状态码`429`，`flow limiting`即限流

![image-20250110114059436](https://s2.loli.net/2025/01/10/URzt2jTC3ZGNhdo.png)

#### 2.并发线程数

> 并发线程数是指在某一时刻同时访问同一资源的线程数量。换句话说，就是同时处理该资源逻辑的线程数。
>
> 在 Sentinel 中，通过并发线程数限流可以有效避免某些资源因为过多线程同时访问而导致性能问题（例如：锁竞争、CPU 或内存消耗过高）。

试想这样一个场景：

假设一个服务器并发线程数上限为50，有10个接口，平均每个接口可以连接5个线程。如果不对每个接口进行限制的话，如果此时有一个接口出现问题，处理请求的速度大大降低，源源不断的请求打到这个接口，那么这个接口所需要的并发线程数就会快速增加，影响其他接口的处理速度，严重的话甚至会耗尽整个服务器资源。

**在sentinel中对接口设置并发请求数上限，防止出现问题的接口耗尽服务器资源，实现线程隔离**

![image-20250110122040749](https://s2.loli.net/2025/01/10/hHDMKjp6iUFRedr.png)

## 4.熔断

> 熔断实际上就是断开与异常微服务的连接，我们可以配置一定的规则来判断微服务是否出现异常，如果出现异常就将其熔断，等待一段时间后放行一些流量测试，如果还是异常则继续熔断，直到正常才继续连接，熔断可以防止服务出现异常后还一直消耗资源

熔断状态：

- Closed（关闭）：默认状态，请求正常调用。
- Open（打开）：触发熔断，短时间内拒绝请求。
- Half-Open（半开）：经过一段时间后，断路器尝试恢复，允许部分请求通过测试是否恢复。

触发熔断的条件：

- 失败率阈值：请求失败的比例超过设定值。
- 请求次数：总请求量达到最小阈值后，才开始统计失败率。
- 响应时间阈值：服务响应时间过长。

恢复机制： 熔断后，系统会经过一段时间尝试恢复，允许部分流量进入测试服务是否恢复正常。

![image-20250111153826954](https://s2.loli.net/2025/01/11/UtqKufCalwLTRov.png)

## 5.自定义返回响应

添加一个自定义配置，发生`BlockException`异常后设置状态码为429，并写入自定义的返回数据

```java
/**
 * sentinel配置
 */
@Slf4j
@Configuration
public class SentinelConfig implements BlockExceptionHandler {
    /**
     * 统一处理限流返回响应
     */
    @Override
    public void handle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, BlockException e) throws Exception {
        log.error(E.FLOW_LIMIT, e);
        httpServletResponse.setStatus(429);
        R<Object> error = R.error(429, E.FLOW_LIMIT);
        httpServletResponse.setContentType("application/json;charset=UTF-8");
        httpServletResponse.getWriter().write(JSON.toJSONString(error));
    }
}
```

## 6.sentinel规则持久化

TODO

- [ ] sentinel规则如何持久化



[^sentinel]:https://sentinelguard.io/zh-cn

[^sentinel-release]:https://github.com/alibaba/Sentinel/releases
