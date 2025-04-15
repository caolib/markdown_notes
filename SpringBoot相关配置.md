---
title: springboot相关配置
date: 2024-01-07 13:10:42
categories: 后端
tags: 
  - springboot
---

# springboot相关配置

## 1.[自定义项目LOGO](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.spring-application.banner)

在`resources`文件夹下新建一个`banner.txt`文件，加入相关内容即可，[艺术字生成网站](https://www.bootschool.net/ascii-art)

```txt
---------------+---------------
          ___ /^^[___              _
         /|^+----+   |#___________//
       ( -+ |____|    ______-----+/
        ==_________--'            \
          ~_|___|__
```

## 2.跨域请求

添加此配置到`WebMvc`配置类中(推荐)，也可以在每个`Controller`类上添加注解`@CrossOrigin`

```java
@Configuration
public class WebMvcConfig implements WebMvcConfigurer {
    //允许所有跨域请求
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOriginPatterns("*")
                .allowedMethods("*")
                .allowedHeaders("*")
                .allowCredentials(true);
    }
}
```

## 3.自动重启

使用dev-tools实现项目代码修改后自动重启，**相比于手动重启速度更快**

- 导入依赖，刷新maven，然后重启项目

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <scope>runtime</scope>
    <optional>true</optional>
</dependency>
```

- 在IDEA设置中打开高级设置，搜索编译，打开下图中选项

![image-20241231180541224](https://s2.loli.net/2024/12/31/4JTy5dRIasjcmuQ.png)

- 在左侧打开**服务**工具窗口，右键项目启动类配置，选择**编辑所选配置**

![image-20250108164756438](C:/Users/12655/AppData/Roaming/Typora/typora-user-images/image-20250108164756438.png)

点击**修改选项**

![image-20250108164858397](C:/Users/12655/AppData/Roaming/Typora/typora-user-images/image-20250108164858397.png)

在 **切换出IDE时** 一项中选择 **更新类和资源**，这样只要光标焦点离开IDEA就会自动重新构建并启动项目，速度较快

![image-20250108164933177](C:/Users/12655/AppData/Roaming/Typora/typora-user-images/image-20250108164933177.png)

