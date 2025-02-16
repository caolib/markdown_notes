---
title: springboot发送邮件
date: 2024年3月3日17:45:58
categories: 后端
tags: 
  - 邮件
  - springboot
cover: https://files.codelife.cc/wallhaven/full/3l/wallhaven-3l9z69.png?x-oss-process=image/resize,limit_0,m_fill,w_1920,h_1080/quality,Q_95/format,webp
---

# springboot发送邮件

## 导入依赖

```xml
<!-- 邮件依赖 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```

## 添加配置

```yml
spring:
  mail:
    # qq邮箱的host
    host: smtp.qq.com
    # 端口，固定的
    port: 465
    # 发件人的邮箱
    username: 1265****79@qq.com
    # qq邮箱服务的授权码
    password: etj*******afh
    test-connection: true
    properties:
      mail:
        smtp:
          ssl:
            enable: true
```

## 测试发送

```java
@SpringBootTest
class MailApplicationTests {
    @Autowired
    private JavaMailSender javaMailSender;

    @Test
    public void senderMail() {
        SimpleMailMessage message = new SimpleMailMessage();
        // 发件人 你的邮箱
        message.setFrom("12******9@qq.com");
        // 接收人 接收者邮箱
        message.setTo(new String[]{"tan*****@gmail.com"});
        //邮件主题
        message.setSubject("hello");
        //邮件内容
        message.setText("test");

        javaMailSender.send(message);
    }
}
```
