---
title: springboot文件上传时，出现文件大小超过限制异常
date: 2019-09-04 16:19:40
tags: [Springboot,异常]
---

#### springboot文件上传报org.apache.tomcat.util.http.fileupload.FileUploadBase$FileSizeLimitExceededException: The field file exceeds its maximum permitted size of 1048576 bytes.异常
<!--more-->
### 具体异常内容：
```
org.apache.tomcat.util.http.fileupload.FileUploadBase$FileSizeLimitExceededException: The field file exceeds its maximum permitted size of 1048576 bytes.
	at org.apache.tomcat.util.http.fileupload.FileUploadBase$FileItemIteratorImpl$FileItemStreamImpl$1.raiseError(FileUploadBase.java:633) ~[tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.tomcat.util.http.fileupload.util.LimitedInputStream.checkLimit(LimitedInputStream.java:76) ~[tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.tomcat.util.http.fileupload.util.LimitedInputStream.read(LimitedInputStream.java:135) ~[tomcat-embed-core-9.0.16.jar:9.0.16]
	at java.io.FilterInputStream.read(FilterInputStream.java:107) ~[na:1.8.0_121]
	at org.apache.tomcat.util.http.fileupload.util.Streams.copy(Streams.java:98) ~[tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.tomcat.util.http.fileupload.util.Streams.copy(Streams.java:68) ~[tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.tomcat.util.http.fileupload.FileUploadBase.parseRequest(FileUploadBase.java:293) ~[tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.catalina.connector.Request.parseParts(Request.java:2846) ~[tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.catalina.connector.Request.getParts(Request.java:2754) ~[tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.catalina.connector.RequestFacade.getParts(RequestFacade.java:1098) ~[tomcat-embed-core-9.0.16.jar:9.0.16]
	at javax.servlet.http.HttpServletRequestWrapper.getParts(HttpServletRequestWrapper.java:356) ~[tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.springframework.web.multipart.support.StandardMultipartHttpServletRequest.parseRequest(StandardMultipartHttpServletRequest.java:94) ~[spring-web-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.web.multipart.support.StandardMultipartHttpServletRequest.<init>(StandardMultipartHttpServletRequest.java:87) ~[spring-web-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.web.multipart.support.StandardServletMultipartResolver.resolveMultipart(StandardServletMultipartResolver.java:87) ~[spring-web-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.web.servlet.DispatcherServlet.checkMultipart(DispatcherServlet.java:1175) ~[spring-webmvc-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:1010) ~[spring-webmvc-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:942) ~[spring-webmvc-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:1005) ~[spring-webmvc-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.web.servlet.FrameworkServlet.doPost(FrameworkServlet.java:908) ~[spring-webmvc-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:660) ~[tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:882) ~[spring-webmvc-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:741) ~[tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:231) ~[tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) ~[tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:53) ~[tomcat-embed-websocket-9.0.16.jar:9.0.16]
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) ~[tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) ~[tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.shiro.web.servlet.AbstractShiroFilter.executeChain(AbstractShiroFilter.java:449) ~[shiro-web-1.4.0.jar:1.4.0]
	at org.apache.shiro.web.servlet.AbstractShiroFilter$1.call(AbstractShiroFilter.java:365) ~[shiro-web-1.4.0.jar:1.4.0]
	at org.apache.shiro.subject.support.SubjectCallable.doCall(SubjectCallable.java:90) ~[shiro-core-1.4.0.jar:1.4.0]
	at org.apache.shiro.subject.support.SubjectCallable.call(SubjectCallable.java:83) ~[shiro-core-1.4.0.jar:1.4.0]
	at org.apache.shiro.subject.support.DelegatingSubject.execute(DelegatingSubject.java:387) ~[shiro-core-1.4.0.jar:1.4.0]
	at org.apache.shiro.web.servlet.AbstractShiroFilter.doFilterInternal(AbstractShiroFilter.java:362) ~[shiro-web-1.4.0.jar:1.4.0]
	at org.apache.shiro.web.servlet.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:125) ~[shiro-web-1.4.0.jar:1.4.0]
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) ~[tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) ~[tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.springframework.web.filter.CharacterEncodingFilter.doFilterInternal(CharacterEncodingFilter.java:200) ~[spring-web-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:107) ~[spring-web-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) ~[tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) ~[tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:200) ~[tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:96) [tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.catalina.authenticator.AuthenticatorBase.invoke(AuthenticatorBase.java:490) [tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:139) [tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:92) [tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.catalina.core.StandardEngineValve.invoke(StandardEngineValve.java:74) [tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:343) [tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.coyote.http11.Http11Processor.service(Http11Processor.java:408) [tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.coyote.AbstractProcessorLight.process(AbstractProcessorLight.java:66) [tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.coyote.AbstractProtocol$ConnectionHandler.process(AbstractProtocol.java:834) [tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.tomcat.util.net.NioEndpoint$SocketProcessor.doRun(NioEndpoint.java:1415) [tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.tomcat.util.net.SocketProcessorBase.run(SocketProcessorBase.java:49) [tomcat-embed-core-9.0.16.jar:9.0.16]
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142) [na:1.8.0_121]
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617) [na:1.8.0_121]
	at org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:61) [tomcat-embed-core-9.0.16.jar:9.0.16]
	at java.lang.Thread.run(Thread.java:745) [na:1.8.0_121]
```

### 原因：tomcat最大上传大小不能超过1048576字节。

### 解决方法
在springboot启动类中通过配置bean来修改上传文件大小
```
@Bean
	public MultipartConfigElement multipartConfigElement() {
		MultipartConfigFactory factory = new MultipartConfigFactory();
		// 文件最大
		factory.setMaxFileSize("100MB"); // KB,MB
		/// 设置总上传数据总大小
		factory.setMaxRequestSize("100MB");
		return factory.createMultipartConfig();
	}
```
在启动类中还要加一个@Configuration注解



<!-- # 使用 pv 记录方式，每访问一次，记录一次--> 
<span id="busuanzi_container_page_pv">  本文总阅读量<span id="busuanzi_value_page_pv"></span>次</span>
