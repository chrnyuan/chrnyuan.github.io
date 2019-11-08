---
title: 生成图片验证码Kaptcha
date: 2019-09-09 11:35:26
tags: [验证码,Kaptcha]
categories: "Java"
---


图片验证码一般是为了网站的登录或其他接口被被恶意请求

Google的Kapatcha验证码是一个简单容易上手的验证码工具。
<!--more-->

一：创建springboot项目,pom文件中添加Kaptcha的Maven依赖
```
        <dependency>
            <groupId>com.github.axet</groupId>
            <artifactId>kaptcha</artifactId>
            <version>0.0.9</version>
        </dependency>

```
二：配置Kaptcha的Bean
```
/**
 * 验证码配置
 * @author ZYK
 * 2019年9月9日 上午10:55:38
 * Desc:
 */
@Configuration
public class KaptchaConfig {

	@Bean
	public DefaultKaptcha producer(){
		
		Properties properties = new Properties();
		
		properties.put("kaptcha.border" , "no");//
	    properties.put("kaptcha.textproducer.font.color" , "blue");//设置字体颜色
	    properties.put("kaptcha.textproducer.char.space" , "5");
	    
		DefaultKaptcha defaultKaptcha = new DefaultKaptcha();
		Config config = new Config(properties);
		defaultKaptcha.setConfig(config);
		return defaultKaptcha;
	}
}

```
三：代码实现
```
@RestController
@Slf4j
@AllArgsConstructor//生成一个包含所有参数的构造方法
public class TestController {
//	@Autowired
	private Producer producer;
	/**
     * 使用google的kaptcha生成验证码
     */
    @SneakyThrows
    @RequestMapping("/sys/code")
    public void captcha( HttpServletResponse response) {
        response.setHeader("Cache-Control" , "no-store, no-cache");
        response.setContentType("image/jpeg");
        //生成文字验证码
        String text = producer.createText();
        log.info("==================验证码:{}====================" , text);
        //生成图片验证码
        BufferedImage image = producer.createImage(text);
        ServletOutputStream out = response.getOutputStream();
        ImageIO.write(image, "jpg" , out);
        IOUtils.closeQuietly(out);
    }
	
}
```
后端完成

<!-- # 使用 pv 记录方式，每访问一次，记录一次--> 
<span id="busuanzi_container_page_pv">  本文总阅读量<span id="busuanzi_value_page_pv"></span>次</span>