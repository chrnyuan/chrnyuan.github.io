---
title: shiro Realm中注入userService失败
date: 2019-09-03 13:10:04
tags:  shiro 
---

realm调用userService查询时null?
	userService Bean没有注入到spring容器中。
	<!--more-->

#### 具体异常输出	
```
java.lang.NullPointerException: null
	at com.xxx.application.auth.CustomRealm.doGetAuthenticationInfo(CustomRealm.java:44) ~[classes/:na]
	at org.apache.shiro.realm.AuthenticatingRealm.getAuthenticationInfo(AuthenticatingRealm.java:571) ~[shiro-core-1.4.0.jar:1.4.0]
	at org.apache.shiro.authc.pam.ModularRealmAuthenticator.doSingleRealmAuthentication(ModularRealmAuthenticator.java:180) ~[shiro-core-1.4.0.jar:1.4.0]
	at org.apache.shiro.authc.pam.ModularRealmAuthenticator.doAuthenticate(ModularRealmAuthenticator.java:267) ~[shiro-core-1.4.0.jar:1.4.0]
	at org.apache.shiro.authc.AbstractAuthenticator.authenticate(AbstractAuthenticator.java:198) ~[shiro-core-1.4.0.jar:1.4.0]
	at org.apache.shiro.mgt.AuthenticatingSecurityManager.authenticate(AuthenticatingSecurityManager.java:106) [shiro-core-1.4.0.jar:1.4.0]
	at org.apache.shiro.mgt.DefaultSecurityManager.login(DefaultSecurityManager.java:274) [shiro-core-1.4.0.jar:1.4.0]
	at org.apache.shiro.subject.support.DelegatingSubject.login(DelegatingSubject.java:260) [shiro-core-1.4.0.jar:1.4.0]
	at com.xxx.application.controller.UserTestController.login(UserTestController.java:69) [classes/:na]
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) ~[na:1.8.0_121]
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62) ~[na:1.8.0_121]
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) ~[na:1.8.0_121]
	at java.lang.reflect.Method.invoke(Method.java:498) ~[na:1.8.0_121]
	at org.springframework.web.method.support.InvocableHandlerMethod.doInvoke(InvocableHandlerMethod.java:189) [spring-web-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.web.method.support.InvocableHandlerMethod.invokeForRequest(InvocableHandlerMethod.java:138) [spring-web-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:102) [spring-webmvc-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandlerMethod(RequestMappingHandlerAdapter.java:895) [spring-webmvc-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:800) [spring-webmvc-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.web.servlet.mvc.method.AbstractHandlerMethodAdapter.handle(AbstractHandlerMethodAdapter.java:87) [spring-webmvc-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:1038) [spring-webmvc-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:942) [spring-webmvc-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:1005) [spring-webmvc-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.web.servlet.FrameworkServlet.doPost(FrameworkServlet.java:908) [spring-webmvc-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:660) [tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:882) [spring-webmvc-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:741) [tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:231) [tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) [tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:53) [tomcat-embed-websocket-9.0.16.jar:9.0.16]
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) [tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) [tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.shiro.web.servlet.ProxiedFilterChain.doFilter(ProxiedFilterChain.java:61) [shiro-web-1.4.0.jar:1.4.0]
	at org.apache.shiro.web.servlet.AdviceFilter.executeChain(AdviceFilter.java:108) [shiro-web-1.4.0.jar:1.4.0]
	at org.apache.shiro.web.servlet.AdviceFilter.doFilterInternal(AdviceFilter.java:137) [shiro-web-1.4.0.jar:1.4.0]
	at org.apache.shiro.web.servlet.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:125) [shiro-web-1.4.0.jar:1.4.0]
	at org.apache.shiro.web.servlet.ProxiedFilterChain.doFilter(ProxiedFilterChain.java:66) [shiro-web-1.4.0.jar:1.4.0]
	at org.apache.shiro.web.servlet.AbstractShiroFilter.executeChain(AbstractShiroFilter.java:449) [shiro-web-1.4.0.jar:1.4.0]
	at org.apache.shiro.web.servlet.AbstractShiroFilter$1.call(AbstractShiroFilter.java:365) [shiro-web-1.4.0.jar:1.4.0]
	at org.apache.shiro.subject.support.SubjectCallable.doCall(SubjectCallable.java:90) [shiro-core-1.4.0.jar:1.4.0]
	at org.apache.shiro.subject.support.SubjectCallable.call(SubjectCallable.java:83) [shiro-core-1.4.0.jar:1.4.0]
	at org.apache.shiro.subject.support.DelegatingSubject.execute(DelegatingSubject.java:387) [shiro-core-1.4.0.jar:1.4.0]
	at org.apache.shiro.web.servlet.AbstractShiroFilter.doFilterInternal(AbstractShiroFilter.java:362) [shiro-web-1.4.0.jar:1.4.0]
	at org.apache.shiro.web.servlet.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:125) [shiro-web-1.4.0.jar:1.4.0]
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) [tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) [tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.springframework.web.filter.CharacterEncodingFilter.doFilterInternal(CharacterEncodingFilter.java:200) [spring-web-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:107) [spring-web-5.1.5.RELEASE.jar:5.1.5.RELEASE]
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) [tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) [tomcat-embed-core-9.0.16.jar:9.0.16]
	at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:200) [tomcat-embed-core-9.0.16.jar:9.0.16]
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
解决方法
直接在shiroconfig中修改
原来的shiroconfig
```
	@Bean(name="securityManager")
	public SecurityManager securityManager(){
		DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
		//将自定义的Realm 交给安全管理器管理
		securityManager.setRealm(new CustomRealm());
		return securityManager;
	}
```
修改后

```
	@Bean(name="securityManager")
	public SecurityManager securityManager(@Qualifier("customRealm") CustomRealm customRealm){
		DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
		//将自定义的Realm 交给安全管理器管理
		securityManager.setRealm(customRealm);
		return securityManager;
	}
	
	@Bean(name="customRealm")
	public CustomRealm myRealm(){
		CustomRealm customRealm = new CustomRealm();
		return customRealm;
	}
```

在ShiroConfiguration中要使用@Bean在ApplicationContext注入MyRealm，不能直接new对象。 
道理和Controller中调用Service一样，都要是SpringBean，不能自己new。



<!-- # 使用 pv 记录方式，每访问一次，记录一次--> 
<span id="busuanzi_container_page_pv">  本文总阅读量<span id="busuanzi_value_page_pv"></span>次</span>
