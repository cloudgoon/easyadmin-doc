## 7.2.1.全部方法    :id=method
EmailService已经封装了三个通用的发邮件的方法：

方法 | 说明
:--- | :---
sendTextEmail(String title, String content, String[] toEmails) | 发送普通文本邮件
sendFullTextEmail(String title, String html, String[] toEmails) | 发送富文本邮件
sendHtmlEmail(String title, String htmlPath, Map<String, Object> map, String[] toEmails) | 发送html模板邮件

使用方法：
```java
public class Test {
    @Autowired
    private EmailService emailService;

    public void test() throws MessagingException, IOException {
        // 发送纯文本邮件
        emailService.sendTextEmail("邮件标题", "邮件内容", new String[]{"xxx@qq.com"});
        
        // 发送富文本邮件
        emailService.sendFullTextEmail("邮件标题", "<span style=\"color:red;\">邮件内容</span>", new String[]{"xxx@qq.com"});
    }
}
```


## 7.2.2.发送模板邮件    :id=tpl
```java
public class Test {
    @Autowired
    private EmailService emailService;

    public void test() throws MessagingException, IOException {
        Map<String, Object> map = new HashMap<>();  // 页面的动态数据
        map.put("userAccount", "111111@qq.com");
        map.put("goodsName", "iframe版");
        emailService.sendHtmlEmail("邮件标题", "system/test.html", map, new String[]{"222222@qq.com"});
    }
}
```

这里使用的是beetl的模板引擎解析的，所以在test.html中使用beetl语法获取map里面的数据：
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>EasyWeb订单通知</title>
	<style> /** 你还可以加样式在这里 */ </style>
</head>
<body>
<table class="my-table" style="margin-top: 25px;">
	<colgroup>
		<col width="110px">
	</colgroup>
	<tbody>
		<tr>
			<th>用户账号：</th> <td>${userAccount!}</td>
		</tr>
		<tr>
			<th>购买商品：</th> <td>${goodsName!}</td>
		</tr>
		<tr>
			<th>购买时间：</th> <td>${buyTime!}</td>
		</tr>
	</tbody>
</table>
</body>
</html>
```


## 7.2.3.使用前配置    :id=config

首先把EmailServiceImpl.java里面的发件人改成发件服务器的邮箱地址：
```java
public class EmailServiceImpl implements EmailService {
    private static final String FROM_EMAIL = "xxxx@foxmail.com";  // 发件人
}
```

发件服务器配置，在application.properties中加入：
```
spring.mail.host=smtp.qq.com
# 这里改成你的邮箱
spring.mail.username=xxxxx@foxmail.com
# 这里改成你的密码
spring.mail.password=xxxxxxxxxx
spring.mail.default-encoding=UTF-8
## 使用25端口认证
#spring.mail.port=25
#spring.mail.properties.mail.smtp.auth=true
#spring.mail.properties.mail.smtp.starttls.enable=true
#spring.mail.properties.mail.smtp.starttls.required=true
## 使用465端口SSl认证
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.socketFactory.class=javax.net.ssl.SSLSocketFactory
spring.mail.properties.mail.smtp.socketFactory.port=465
```

这里的码不是登录密码，获取方式如下，以QQ邮箱为例，进入“设置/账户”：
![](https://s2.ax1x.com/2019/06/23/ZPRKrq.png)

找到“POP3/IMAP/SMTP/Exchange/CardDAV/CalDAV服务”：

![](https://s2.ax1x.com/2019/06/23/ZPR6RH.png)

点击“开启”就会弹出密码。

!> 25端口和465端口配置一个即可，EmailServiceImpl里的发件人和application.properties中的配置要一致

