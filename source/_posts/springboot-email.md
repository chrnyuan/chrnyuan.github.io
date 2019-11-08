---
title: springboot_email
date: 2019-10-30 18:12:33
tags: springboot
---
springboot+Mail集成，实现对邮件的发送。主要应用场景：忘记密码，通过邮箱验证码找回等

#### 1：邮箱开启SMTP服务
设置邮箱账号，我使用的是网易邮箱，需要开通SMTP(简单的邮箱传输协议)，
在网易邮箱点击设置-->POP3/SMTP/IMAP--然后开启客户端授权密码
，需要记住此密码

### 2：springboot代码部分
pom文件
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.1.3.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.example</groupId>
	<artifactId>springboot_mail</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>springboot_mail</name>
	<description>Demo project for Spring Boot</description>

	<properties>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-mail</artifactId>
        </dependency>
        
        <dependency>
    		<groupId>org.springframework.boot</groupId>
    		<artifactId>spring-boot-starter-thymeleaf</artifactId>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>
```
application.yml
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.1.3.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.example</groupId>
	<artifactId>springboot_mail</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>springboot_mail</name>
	<description>Demo project for Spring Boot</description>

	<properties>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-mail</artifactId>
        </dependency>
        
        <dependency>
    		<groupId>org.springframework.boot</groupId>
    		<artifactId>spring-boot-starter-thymeleaf</artifactId>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>
```
Controller
```
@RestController
public class MailController {

	@Autowired(required = false)
    private JavaMailSender jms;
	@Autowired
	private TemplateEngine templateEngine;
	
    @Value("${spring.mail.username}")
    private String from;
    
    
    String targetMail = "888888@qq.com";
    /**
     * 发送简单的邮件
     * @return
     */
    @RequestMapping("sendSimpleEmail")
    public String sendMail(){
       try{
           SimpleMailMessage message = new SimpleMailMessage();
           message.setFrom(from);
           message.setTo(targetMail);//接收邮件号
           message.setSubject("一封简单地邮件");//邮件主题
           message.setText("Hello World");//邮件内容
           jms.send(message);
           return "发送成功";
       }catch (Exception e){
           e.getStackTrace();
           return e.getMessage();
       }

    }
    /**
     * 发送HTML格式文件
     * @return
     */
    @RequestMapping("sendHtmlEmail")
    public String sendHtmlEmail(){
    	 MimeMessage message = null;
         try {
             message = jms.createMimeMessage();
           //发送邮件之间给自己抄送一份，防止网易当做垃圾邮件
             message.addRecipients(MimeMessage.RecipientType.CC, InternetAddress.parse(from));
             MimeMessageHelper helper = new MimeMessageHelper(message, true);
             helper.setFrom(from); 
             helper.setTo(targetMail); // 接收地址
             helper.setSubject("一封HTML格式的邮件"); // 标题
             // 带HTML格式的内容
             StringBuffer sb = new StringBuffer("<p style='color:#42b983'>使用Spring Boot发送HTML格式邮件。</p>");
             helper.setText(sb.toString(), true);
             jms.send(message);
             return "发送成功";
         } catch (Exception e) {
             e.printStackTrace();
             return e.getMessage();
         }
    	
    }
    /**
     * 发送带附件的邮件
     * @return
     */
    @RequestMapping("sendAttachmentsMail")
    public String sendAttachmentsMail() {
        MimeMessage message = null;
        try {
            message = jms.createMimeMessage();
            MimeMessageHelper helper = new MimeMessageHelper(message, true);
          //发送邮件之间给自己抄送一份，防止网易当做垃圾邮件
            message.addRecipients(MimeMessage.RecipientType.CC, InternetAddress.parse(from));
            helper.setFrom(from); 
            helper.setTo(targetMail); // 接收地址
            helper.setSubject("一封带附件的邮件"); // 标题
            helper.setText("详情参见附件内容！"); // 内容
            // 传入附件
            FileSystemResource file = new FileSystemResource(new File("src/main/resources/static/file/项目文档.docx"));
            helper.addAttachment("项目文档.docx", file);
            jms.send(message);
            return "发送成功";
        } catch (Exception e) {
            e.printStackTrace();
            return e.getMessage();
        }
    }
    /**
     * 发送模板邮件
     * @param code
     * @return
     */
    @RequestMapping("sendTemplateEmail")
    public String sendTemplateEmail(String code) {
        MimeMessage message = null;
        try {
            message = jms.createMimeMessage();
            //发送邮件之间给自己抄送一份，防止网易当做垃圾邮件
            message.addRecipients(MimeMessage.RecipientType.CC, InternetAddress.parse(from));
            MimeMessageHelper helper = new MimeMessageHelper(message, true);
            helper.setFrom(from); 
            helper.setTo(targetMail); // 接收地址
            helper.setSubject("邮件摸板测试"); // 标题
            // 处理邮件模板
            Context context = new Context();
            context.setVariable("code", code);
            String template = templateEngine.process("emailTemplate", context);
            helper.setText(template, true);
            jms.send(message);
            return "发送成功";
        } catch (Exception e) {
            e.printStackTrace();
            return e.getMessage();
        }
    }

}
```

具体代码地址:https://gitee.com/chrnyuan/springboot_email.git

<!-- # 使用 pv 记录方式，每访问一次，记录一次--> 
<span id="busuanzi_container_page_pv">  本文总阅读量<span id="busuanzi_value_page_pv"></span>次</span>