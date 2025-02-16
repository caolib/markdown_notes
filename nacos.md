---
title: nacos从入门到入土
date: 2024-03-16 18:42:42
categories: 后端
tags: 
  - springcloud
  - nacos
cover: https://files.codelife.cc/wallhaven/full/mp/wallhaven-mpjj91.jpg?x-oss-process=image/resize,limit_0,m_fill,w_1920,h_1080/quality,Q_95/format,webp
---

# nacos

## 注册中心

### 1. 下载

[Nacos Server 下载 | Nacos](https://nacos.io/download/nacos-server/#稳定版本)

### 2. 启动

> 解压缩后在`bin`目录下有几个脚本，`startup`就是启动脚本,默认都是集群`cluster`方式启动,也可以使用单机`standalone`模式启动

```sh
# 单机模式启动 
./startup.cmd -m standalone

# 集群模式启动
./startup.cmd
```

![image-20241226192616560](https://s2.loli.net/2024/12/26/9DLP4RrzJdsmkIw.png)

> [!note] 
>
> 启动后默认端口`8848`，直接在浏览器打开<http://localhost:8848/nacos> 进行访问，默认用户名和密码都是`nacos`可以在`conf/application.properties`文件中修改端口`server.port`

![image-20241226193352670](https://s2.loli.net/2024/12/26/Ol378APSIxhCJie.png)

### 3. 导入依赖

```xml
<!--nacos-->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
```

版本号可以指定也可以不指定，**如果不指定的话，需要导入下面的依赖管理**，添加了这个配置后，spring-cloud-alibaba的依赖版本都使用这个版本，便于统一版本

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-dependencies</artifactId>
            <version>2.2.5.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

### 3. 添加配置

添加nacos地址与当前项目的应用名称，默认是单机模式

```yml
spring:
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848
  application:
    name: ms-user
```

### 4. 注册服务

启动项目，可以看到当前项目服务已经注册到nacos

![image-20241226200052783](https://s2.loli.net/2024/12/26/KWIJXuB25kav4TV.png)

## 配置中心

### 1. 引入依赖

在`pom.xml`文件中引入nacos配置管理的依赖

```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
</dependency>
```

### 2. 添加配置

- 在`resources`文件夹下新建配置文件`bootstrap.yml`文件，添加nacos的**配置地址**，注意与前面的**注册发现地址**区分

```yml
spring:
  application:
    name: ms-coupon
  cloud:
    nacos:
      server-addr: localhost:8848
```

- 在`application.yml`中添加自定义配置

```yml
# 配置属性
student:
  name: zhangsan
  age: 18
```

- 新建对应的配置类，使用`@ConfigurationProperties`注解读取配置值

```java
@Data
@Component
@ConfigurationProperties(prefix = "student")
public class Student {
    private String name;
    private Integer age;
}
```

### 3. 测试接口

在controller层新增接口，并注入刚才的属性值

```java
@RestController
@RequiredArgsConstructor
public class CouponController {
    private final Student student; // 属性值来自application.yml的值
    @RequestMapping("/test")
    public R test() {
        return R.ok().put("name", student.getName()).put("age", student.getAge());
    }
}
```

### 4. 测试接口

根据接口路径发送对应请求，可以获取到对应的数据

![image-20241226202416312](https://s2.loli.net/2024/12/26/AMzkYpxlV4173dE.png)

### 5. 热更新配置

> 问题：如果配置的值需要变更，那么我就需要修改项目中的配置文件，然后重新启动项目，**非常麻烦且耗费时间**
>
> 解决：
>
> 1. - nacos配置文件格式为：**[spring.application.name]-(dev|prod|local).[文件后缀]**三部分
>    - 中间的作用范围可选，**Data ID**可以省略文件后缀，直接填微服务名
>    - 注意配置格式(文件后缀)对应你填写配置时用的格式
>
>    <img src="https://s2.loli.net/2024/12/26/He3P6mlYACUERyL.png" alt="image-20241226223313483" style="zoom:50%;" />
>
> 2. 填写后点击发布，重新测试接口，数据发生变化而无需重启项目(配置中心的优先级比项目中的配置更高)
>
> 3. 以后需要修改配置，只需要在nacos控制台修改配置文件后就能热更新了

## 命名空间

### 命名空间

> **Nacos的命名空间用于配置隔离**，Nacos有一个默认的命名空间`public`
>
> 1. 我们可以**为每个微服务创建对应的命名空间**实现**微服务之间的配置隔离**，也可以基于环境进行隔离（dev、test、prod）
>
> 2. 例如分别为user，coupon微服务创建user、coupon命名空间，创建完命名空间后会生成对应的**命名空间ID**
>
> 3. 复制命名空间ID，在对应的`bootstrap.yml`添加配置`namespace`属性配置，然后重启项目
>
>    ```yml
>    spring:
>      application:
>        name: ms-coupon
>      cloud:
>        nacos:
>          server-addr: localhost:8848
>          config:
>            namespace: 7a2cc573-0516-4655-8c69-525496095cfe
>    ```

![image-20241231164449369](https://s2.loli.net/2024/12/31/y24IBPFDMid7keS.png)

在配置列表将public下的配置文件克隆到coupon下(也可以手动创建)

![image-20241231165831119](https://s2.loli.net/2024/12/31/D38RTkwmPdIBWux.png)

然后修改配置，点击发布后重新测试，发现此时使用的是coupon环境下的配置

```properties
student.name=zs-coupon
student.age=22
```

### 配置

#### 配置集ID

即`Data ID`,其实就是配置文件名

#### 配置分组

配置文件所属的分组，**同样用于配置隔离**，默认所有配置文件都属于`DEFAULT_GROUP`分组

![image-20241231181035617](https://s2.loli.net/2024/12/31/8i2gcJe1XVxIDGt.png)

### 如何使用

怎么使用命名空间和配置分组进行各个微服务的配置隔离和不同环境的配置隔离？

> [!important]
>
> 1. **为每个微服务创建对应的命名空间**实现**微服务之间的配置隔离**
> 2. 在每个微服务的**命名空间下创建多个分组**实现**多环境的配置隔离**

![image-20241231171938560](https://s2.loli.net/2024/12/31/1HCIgZKY2pchkSR.png)

### 多配置文件

> [!important]
>
> 当配置越来越多后，所有配置都放在`application.yml`中显得有些臃肿，我们可以把配置拆分为多个配置，在nacos分别创建对应的配置文件

创建数据源配置 `datasource.yml`

```yml
# 数据源配置
spring:
  datasource:
    username: root
    password: root
    url: jdbc:mysql://centos:3306/coupon?useSSL=false&serverTimezone=Asia/Shanghai
    driver-class-name: com.mysql.cj.jdbc.Driver
```

创建mybatis配置 `mybatis.yml`

```yml
# mybatis-plus配置
mybatis-plus:
  mapper-locations: classpath*:/mapper/**/*.xml
  global-config:
    db-config:
      id-type: auto
```

创建其他配置 `others.yml`

```yml
# 优惠券
server:
  port: 12000
spring:
  application:
    name: ms-coupon
  cloud:
    # nacos配置
    nacos:
      discovery:
        server-addr: localhost:8848
  # 热更新
  devtools:
    restart:
      additional-paths: src/main
      additional-exclude: src/main/resources/static/**
# 自定义配置属性
student:
  name: zs
  age: 18
```

![image-20241231182304828](https://s2.loli.net/2024/12/31/pFdJw1kBP8273i4.png)

在项目的`bootstrap.yml`文件中添加这些配置文件，项目启动后自动从nacos获取对应配置，优先级高于项目的`application.yml`配置（其实本地的配置删了都没问题，因为配置可以从nacos拉取）

```yml
spring:
  cloud:
    nacos:
      config:
        # 添加这3个配置文件的相应配置
        extension-configs[0]:
            data-id: datasource.yml
            refresh: true
            group: dev
        extension-configs[1]:
            data-id: mybatis.yml
            refresh: true
            group: dev
        extension-configs[2]:
            data-id: others.yml
            refresh: true
            group: dev
```

