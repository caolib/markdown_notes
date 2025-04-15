---
title: Gateway
tags:
  - gateway
date: 2025-01-01 13:37:56
categories:
  - springcloud
cover: https://s2.loli.net/2025/01/01/8eLKDNqIJkwv6ph.webp
---

# Gateway

[toc]

## 1.概述

### 是什么？

> Spring Cloud Gateway 是一个基于 Spring Framework 和 Spring Boot 构建的 <span alt='highlight'>API 网关框架</span>，用于**路由**和**负载均衡**。

### 有什么用？

![](https://docs.spring.io/spring-cloud-gateway/reference/_images/spring_cloud_gateway_diagram.png)

1. **路由功能**：`Spring Cloud Gateway` 通过配置的路由规则，将外部请求转发到后端服务。这些路由可以基于请求的 URL、HTTP 方法、请求头等进行匹配。
2. **负载均衡**：`Spring Cloud Gateway` 可以与 `Spring Cloud LoadBalancer` 配合，进行请求的负载均衡，确保请求的流量均衡地分配到各个后端服务。
3. **过滤器**：可以使用过滤器进行请求和响应的**预处理或后处理**，例如认证、日志记录、请求转发等。
4. **动态路由**：支持根据条件（如用户身份、请求参数等）动态修改路由规则。
5. **集成其他 Spring Cloud 组件**：Spring Cloud Gateway 与其他 Spring Cloud 项目（如 Spring Cloud Config、Spring Cloud Discovery、Spring Security 等）无缝集成，能够帮助实现微服务架构中的服务发现、配置管理、安全认证等功能。

**应用场景：**

- **微服务架构**：作为微服务架构中的 API 网关，Spring Cloud Gateway 可以对外提供统一的入口，简化客户端的访问，同时还能够处理一些常见的网关功能，如身份验证、请求限流等。
- **统一路由管理**：在系统中统一管理所有服务的路由规则，简化了客户端的访问和服务间的通信。
- **负载均衡和流量控制**：配合负载均衡和限流功能，提高服务的可用性和稳定性。

### 相关术语

- **Route**: 路由，网关的基本构建块。它由一个 **ID**、一个目标 **URI**、一组 **谓词**（predicates）和一组 **过滤器**（filters）定义。如果聚合后的谓词为真，则匹配该路由。

- **Predicate**: 断言，这是一个 Java 8 的 **Function Predicate**。其输入类型是 Spring Framework 的 **ServerWebExchange**。这使得你可以基于 HTTP 请求中的任何内容进行匹配，例如请求头或请求参数。

- **Filter**: 过滤器，这些是由特定工厂构造的 **GatewayFilter** 实例。你可以在发送下游请求之前或之后，修改请求和响应。

## 2.快速开始

### 1.导入依赖

要**通过网关将请求路由到其他微服务**，同样需要**将网关注册到注册中心**，以nacos为例，还需要导入nacos注册发现依赖

```xml
<!-- gateway网关 -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>
<!-- nacos注册发现 -->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
<!-- nacos配置 -->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
</dependency>
```



> [!WARNING]
>
> **常见问题**：Gateway微服务启动时出现：**Spring MVC found on classpath, which is incompatible with Spring Cloud Gateway.**
>
> 原因：Spring Cloud Gateway 基于 **Reactive Web**（响应式编程）架构，而 **Spring MVC** 是基于传统的 **Servlet**（阻塞式编程）架构。这两者在同一个应用中**无法共存**，因此会出现冲突

你需要将gateway项目中的`spring-boot-starter-web`依赖删除，或者通过下面的办法排除依赖，因为可能**你没有显式引入MVC依赖，而是因为引入的其他依赖中包含了mvc依赖**

- 在maven工具窗口找到gateway项目，右键**依赖项**，点击**分析依赖关系**

![image-20250101184836544](https://s2.loli.net/2025/01/01/jXYHuesf3F1T9tl.png)

- 在搜索框中搜索**mvc**，找到 `spring-boot-starter-web` 依赖，发现在引入的common模块中

  ![image-20250101185714684](https://s2.loli.net/2025/01/01/HVWfdLe6kcZy5qp.png)

- 找到common依赖，排除整个web依赖或者排除spring-webmvc也可以，这里直接排除整个web依赖，刷新maven后重启项目就可以了

  ```xml
  <dependency>
      <groupId>io.github.caolib</groupId>
      <artifactId>common</artifactId>
      <version>0.0.1-SNAPSHOT</version>
      <!-- 排除spring-web -->
      <exclusion>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-web</artifactId>
      </exclusion>
  </dependency>
  ```

### 2.配置路由

```yml
spring:
  application:
    name: gateway
  cloud:
    gateway:
      # 路由规则
      routes:
        - id: admin-route
          uri: lb://admin
          predicates:
            - Path=/admins/**
        - id: user-route
          uri: lb://user
          predicates:
            - Path=/users/**,/books/**
          filters:
            - AddRequestHeader=X-Request-red, blue
      # 默认过滤器
      default-filters:
        - AddRequestHeader=X-Request-red, blue
```

routes下可以定义多个路由规则，**以第二条路由规则`user-route`为例**

- id：**路由规则的唯一标识**，可以随意自定义但要保证唯一，此处的`user-route`就是id

- uri：**转发的目的地址**，`lb`表示负载均衡，后接转发的微服务名称，此处的 `lb://user` 意思就是从nacos中找到名为user的微服务并将请求转发给它，如果有多个user微服务，则进行负载均衡，uri也可以填固定的地址，比如 `https://example.org`
- predicates：**断言，其实说是转发条件更为合适**，断言中包含各种条件，只有满足这些条件时才会进行转发，此处的 `- Path=/users/**,/books/**` 表示请求路径以 `/users/`或者`/books` 开头的所有请求都会进行转发，像这样的断言规则当然是不止一个，这是[官网所有断言规则](https://docs.spring.io/spring-cloud-gateway/reference/spring-cloud-gateway/request-predicates-factories.html)
- filters：**过滤器**，可以在路由转发请求之前完成一些统一的动作，此处的 `- AddRequestHeader=X-Request-red, blue` 指添加一个请求头。使用 `default-filters` 属性可以配置一个默认的过滤器，所有请求都会添加上这个请求头。像这样的过滤器当然是不止一个，还有很多可以完成其他动作的过滤器，这是[官网所有过滤器](https://docs.spring.io/spring-cloud-gateway/reference/spring-cloud-gateway/gatewayfilter-factories.html)，

## 3.自定义过滤器

### 1.全局过滤器

#### 添加过滤器

```java
@Slf4j
@Component
public class DefaultGlobalFilter implements GlobalFilter, Ordered {
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        ServerHttpRequest request = exchange.getRequest();
		log.debug("request headers: {}", request.getHeaders()); // 打印所有请求头
        log.debug("request path: {}", request.getPath()); 		// 打印请求路径
        // 放行
        return chain.filter(exchange);
    }

    // 过滤器的优先级，数字越小，优先级越高
    @Override
    public int getOrder() {
        return 100;
    }
}
```

自定义一个拦截器实现登录校验，请求发送过来后，网关完成登录校验后将用户信息保存到请求头中，然后转发请求到其他微服务

```java
@Component
@RequiredArgsConstructor
public class LoginFilter implements GlobalFilter, Ordered {
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        // 获取请求路径
        ServerHttpRequest request = exchange.getRequest();
        RequestPath path = request.getPath();

        // 如果请求路径不需要校验直接放行
        // code here ......

        // 取出请求头中的token
        String token = request.getHeaders().getFirst("Authorization");
        // 校验jwt令牌
        // code here ......

        // 将用户信息放入请求头
        ServerWebExchange newRequest = exchange.mutate()
                .request(builder -> builder.header("info","用户信息的字符串")) //这里是怎么存的，到时候就怎么取
                .build();

        return chain.filter(newRequest); // 放行
    }

    @Override
    public int getOrder() {
        return 0;
    }
}
```

问题：网关路由转发的请求中的请求头中的信息**怎么在下游的微服务中获取**呢？

我们可以在其他微服务中**使用拦截器获取到请求头中的用户信息然后保存到ThreadLocal中**，因为每个微服务都需要这个拦截器，所以可以将这个配置放在通用模块中

#### 添加拦截器

```java
public class UserInfoInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
        // 从请求头中获取用户id
        String userId = request.getHeader("info");
        if (StrUtil.isNotBlank(userId))
            // 不为空则保存到ThreadLocal中
            // code here ......
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) {
        // 从ThreadLocal中删除信息
        // code here ......
    }
}
```

#### 添加配置

> [!warning]
>
>  在mvc配置类中添加这个拦截器，此处的 `@ConditionalOnClass(DispatcherServlet.class)` 注解可以防止gateway项目导入这个配置，因为gateway中一般不允许mvc存在，所以自然就没有mvc的依赖，如果导入这个配置就会出错

```java
@Configuration
@ConditionalOnClass(DispatcherServlet.class) // 项目中有mvc的情况下才生效，避免webflux项目报错
public class MvcConfig implements WebMvcConfigurer {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        // 添加用户拦截器
        registry.addInterceptor(new UserInfoInterceptor());
    }
}
```

