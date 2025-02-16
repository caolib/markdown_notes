---
title: 邮箱开启SMTP服务
date: 2024-12-27 20:43:00
categories: tools
tags: 
  - SMTP
  - email
cover: https://files.codelife.cc/wallhaven/full/57/wallhaven-571je1.jpg?x-oss-process=image/resize,limit_0,m_fill,w_1920,h_1080/quality,Q_95/format,webp
---

# 开启邮件SMTP服务

## 1. SMTP服务是什么？

> **SMTP**（Simple Mail Transfer Protocol，简单邮件传输协议）是用于发送和接收电子邮件的通信协议，是电子邮件系统中最基础的协议之一。它负责在邮件服务器之间传输邮件，但在客户端中，通常只用于**发送邮件**。
>
> 简单来说，**SMTP服务的作用是发送电子邮件**，而开启SMTP服务可以让我们使用编程语言发送邮件

## 2. 开启SMTP服务

> 以QQ邮箱为例，登陆[QQ邮箱](https://mail.qq.com)，点击界面右上角**账号与安全**，也可能需要先点击右上角头像然后点击**账号与安全**(qq邮箱如果绑定了微信则界面会不一样)
>
> 随后点击 **安全设置** -> **开启服务**

 ![image-20241227210629985](https://s2.loli.net/2024/12/27/xIj8fl9Qeqnd5tr.png)

> 微信扫码发送短信后点击**我已发送**

![image-20241227210750815](https://s2.loli.net/2024/12/27/dkt9EBzFcDMAOKh.png)

> 复制授权码(保存好这个授权码)，然后点击返回就完成了

![image-20241227211012501](https://s2.loli.net/2024/12/27/xiWaF7dANyJb8kU.png)



## 3. 发送邮件

### 普通文本

> 以python为例发送一个电子邮件，注意修改发、收件人和授权码

```python
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart

smtp_server = 'smtp.qq.com'
smtp_port = 465
sender_email = '2*****76@qq.com'  # 发件人
receiver_email = '2*****76@qq.com' # 收件人
password = '修改为你的授权码' # 授权码

subject = 'Test Email' # 主题
body = 'This is a test email sent using Python.' # 正文

msg = MIMEMultipart()
msg['From'] = sender_email
msg['To'] = receiver_email
msg['Subject'] = subject

msg.attach(MIMEText(body, 'plain'))

# Send the email
try:
    server = smtplib.SMTP_SSL(smtp_server, smtp_port)
    server.login(sender_email, password)
    server.sendmail(sender_email, receiver_email, msg.as_string())
    server.quit()
    print('Email sent successfully!')
except Exception as e:
    print(f'Failed to send email: {e}')
```

![image-20241227212000262](https://s2.loli.net/2024/12/27/xvLgBjfYducsmoE.png)

### HTML内容

> 也可以发送html格式的文本内容，以表格为例,将正文改为表格，然后将MIMEText格式改为html就可以了

```python
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart

smtp_server = 'smtp.qq.com'
smtp_port = 465
sender_email = '230453176@qq.com' 
receiver_email = '230453176@qq.com' 
password = 'jlegvgancaltbijc' 


subject = 'Test Email'
body = '''
<html>
  <body>
    <h2>芝士表格</h2>
    <table border="1" cellpadding="5" cellspacing="0">
      <tr>
        <th>姓名</th>
        <th>年龄</th>
      </tr>
      <tr>
        <td>clb</td>
        <td>18</td>
      </tr>
      <tr>
        <td>zs</td>
        <td>20</td>
      </tr>
    </table>
  </body>
</html>
''' # HTML 正文

msg = MIMEMultipart()
msg['From'] = sender_email
msg['To'] = receiver_email
msg['Subject'] = subject

msg.attach(MIMEText(body, 'html')) # 格式改为html

# Send the email
try:
    server = smtplib.SMTP_SSL(smtp_server, smtp_port)
    server.login(sender_email, password)
    server.sendmail(sender_email, receiver_email, msg.as_string())
    server.quit()
    print('Email sent successfully!')
except Exception as e:
    print(f'Failed to send email: {e}')
```

![image-20241227212648499](https://s2.loli.net/2024/12/27/LVN37hY4DyFa2dq.png)

















