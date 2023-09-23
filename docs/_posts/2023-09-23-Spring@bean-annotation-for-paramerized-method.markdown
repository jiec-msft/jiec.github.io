---
layout: post
title:  "Spring@Bean注解修饰带参数方法"
date:   2023-09-23 15:49:00 +0800
categories: Spring
---

From: https://ellsom1945.github.io/blog/springboot02.html

```Java
/**
    * 声明队列交换机等
    * @param connectionFactory
    * @return
    */
@Bean
public RabbitAdmin rabbitAdmin(ConnectionFactory connectionFactory) {
    System.out.println(String.format("-----------getRabbitAdmin:%s", connectionFactory.hashCode()));
    return new RabbitAdmin(connectionFactory);
}
```

有参数connectFactory，若spring容器中只有一个ConnectionFactory 类型的bean，则不论参数取名为何都是按类型取bean ConnectionFactory 为参数，若有多个则参数取名必须为多个bean中的一个，否则报错。

在SpringBoot中利用这一自动装配特性实现了对用户配置的兼容：

```Java
@Bean
@ConditionalOnBean(MultipartResolver.class)  //容器中有这个类型组件
@ConditionalOnMissingBean(name = DispatcherServlet.MULTIPART_RESOLVER_BEAN_NAME) //容器中没有这个名字 multipartResolver 的组件
public MultipartResolver multipartResolver(MultipartResolver resolver) {
	//给@Bean标注的方法传入了对象参数，这个参数的值就会从容器中找。
	//SpringMVC multipartResolver。防止有些用户配置的文件上传解析器不符合规范
	// Detect if the user has created a MultipartResolver but named it incorrectly
	return resolver;//给容器中加入了文件上传解析器；
}
```

在用户配置multipartResolver，且未正确命名时，将这一对象封装重新注册。