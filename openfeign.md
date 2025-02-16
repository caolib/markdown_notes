---
title: openfeign远程调用
date: 2024-12-26 09:44:56
categories: 后端
tags: 
  - springcloud
  - openfeign
cover: https://files.codelife.cc/wallhaven/full/r2/wallhaven-r2g7rm.jpg
---

# [openfeign](https://docs.spring.io/spring-cloud-openfeign/docs/current/reference/html/)

## 1.feign是什么？

> Feign 是一个声明式的 Web 服务客户端。它使得编写 Web 服务客户端变得更加容易。要使用 Feign，你需要创建一个接口并对其进行注解。它支持可插拔的注解，包括 Feign 注解和 JAX-RS 注解。Feign 还支持可插拔的编码器和解码器。Spring Cloud 增加了对 Spring MVC 注解的支持，并允许使用 Spring Web 默认使用的相同 HttpMessageConverters。Spring Cloud 集成了 Eureka、Spring Cloud CircuitBreaker，以及 Spring Cloud LoadBalancer，以在使用 Feign 时提供负载均衡的 HTTP 客户端。

## 2.快速使用

1. 导入openfeign和负载均衡器依赖

   ```xml
   <!--openfeign-->
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-openfeign</artifactId>
   </dependency>
   <!--负载均衡器-->
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-loadbalancer</artifactId>
   </dependency>
   ```

2. 开启openfeign

   ```java
   @SpringBootApplication
   @EnableFeignClients  // 添加注解开启openfeign
   public class Application {
       public static void main(String[] args) {
           SpringApplication.run(Application.class, args);
       }
   }
   ```

3. 新增接口`/feign/StoreClient.java`，添加调用方法声明(可直接从被调用服务的controller层复制)

   ```java
   @FeignClient("stores") // 调用 stores微服务的方法
   public interface StoreClient {
       // 可以直接将stores中的controller方法的声明直接复制到这里
       @RequestMapping(method = RequestMethod.GET, value = "/stores")
       List<Store> getStores();
   
       @RequestMapping(method = RequestMethod.GET, value = "/stores")
       Page<Store> getStores(Pageable pageable);
   
       @RequestMapping(method = RequestMethod.DELETE, value = "/stores/{storeId:\\d+}")
       void delete(@PathVariable Long storeId);
   }
   ```

4. 如果调用方和被调用方都已经在注册中心(Nacos/Eureka)注册了，接下来就可以直接调用接口进行测试了

## 3.连接池

> 连接池有什么用？在服务访问比较频繁的情况下，**每次访问都要先建立和销毁连接**，开销比较大，连接池可以帮我们管理连接，建立成功的连接**使用完后不会立即释放**（keep-alive），如果有相同的访问请求就可以**基于这个已经建立好的连接**，减少了资源开销和访问时间。
>
> openfeign的默认httpclient不支持连接池，支持连接池的httpclient有`Apache HttpClient`和`OkHttp`两种

### 1.Apache HttpClient

引入依赖

```xml
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient</artifactId>
</dependency>
```

添加配置

```yml
feign:
  httpclient:
    enabled: true  # 启用 Apache HttpClient
```

### 2.OkHttp

引入依赖

```xml
<dependency>
    <groupId>io.github.openfeign</groupId>
    <artifactId>feign-okhttp</artifactId>
</dependency>
```

添加配置

```yml
feign:
  okhttp:
    enabled: true # 启用 OkHttp
```

## 4.日志配置

创建一个feign的配置类，创建一个bean，返回值类型是日志级别，有 `NONE`,`BASIC`,`HEADERS`,`FULL`这几个级别

```java
public class FeignConfig {
    @Bean
    public Logger.Level feignLoggerLevel() {
        return Logger.Level.HEADERS; // openfeign 日志级别
    }
}
```

在启动类上开启feign功能的注解上加载这个配置 `@EnableFeignClients(defaultConfiguration = FeignConfig.class) `
