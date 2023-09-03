---
layout: post
title:  "Java class loading order with Tomcat"
date:   2023-09-03 20:48:00 +0800
categories: Html
---


# Tomcat Java类加载顺序

## 如何查看当前类在JVM中的加载顺序?
使用`-verbose:class` JVM 参数, 使用方式可以用
- `java -verbose:class -jar /path/to/your/jar`
- `export JAVA_OPT="-verbose:class:${JAVA_OPT}"`

例子:
```
[0.625s][info][class,load] org.springframework.context.event.ContextRefreshedEvent source: jar:file:/C:/Users/jiec/devops/projects/personal/mason-learn/web-app-1/target/web-app-1-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/spring-context-6.0.11.jar!/
[0.625s][info][class,load] org.springframework.boot.logging.LoggingSystem source: jar:file:/C:/Users/jiec/devops/projects/personal/mason-learn/web-app-1/target/web-app-1-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/spring-boot-3.1.3.jar!/
[0.626s][info][class,load] org.springframework.boot.logging.LoggingSystem$NoOpLoggingSystem source: jar:file:/C:/Users/jiec/devops/projects/personal/mason-learn/web-app-1/target/web-app-1-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/spring-boot-3.1.3.jar!/
[0.626s][info][class,load] org.springframework.boot.logging.LoggingSystemFactory source: jar:file:/C:/Users/jiec/devops/projects/personal/mason-learn/web-app-1/target/web-app-1-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/spring-boot-3.1.3.jar!/
[0.626s][info][class,load] org.springframework.boot.logging.DelegatingLoggingSystemFactory source: jar:file:/C:/Users/jiec/devops/projects/personal/mason-learn/web-app-1/target/web-app-1-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/spring-boot-3.1.3.jar!/
[0.627s][info][class,load] org.springframework.boot.logging.LoggingSystemFactory$$Lambda$62/0x0000021dae091180 source: org.springframework.boot.logging.LoggingSystemFactory
[0.627s][info][class,load] org.springframework.boot.logging.logback.LogbackLoggingSystem$Factory source: jar:file:/C:/Users/jiec/devops/projects/personal/mason-learn/web-app-1/target/web-app-1-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/spring-boot-3.1.3.jar!/
[0.628s][info][class,load] org.springframework.beans.factory.aot.BeanFactoryInitializationAotProcessor source: jar:file:/C:/Users/jiec/devops/projects/personal/mason-learn/web-app-1/target/web-app-1-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/spring-beans-6.0.11.jar!/
[0.629s][info][class,load] org.springframework.boot.logging.AbstractLoggingSystem source: jar:file:/C:/Users/jiec/devops/projects/personal/mason-learn/web-app-1/target/web-app-1-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/spring-boot-3.1.3.jar!/
[0.629s][info][class,load] org.springframework.boot.logging.logback.LogbackLoggingSystem source: jar:file:/C:/Users/jiec/devops/projects/personal/mason-learn/web-app-1/target/web-app-1-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/spring-boot-3.1.3.jar!/
[0.629s][info][class,load] org.springframework.boot.logging.log4j2.Log4J2LoggingSystem$Factory source: jar:file:/C:/Users/jiec/devops/projects/personal/mason-learn/web-app-1/target/web-app-1-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/spring-boot-3.1.3.jar!/
[0.630s][info][class,load] org.springframework.boot.logging.log4j2.Log4J2LoggingSystem source: jar:file:/C:/Users/jiec/devops/projects/personal/mason-learn/web-app-1/target/web-app-1-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/spring-boot-3.1.3.jar!/
[0.631s][info][class,load] org.springframework.boot.logging.java.JavaLoggingSystem$Factory source: jar:file:/C:/Users/jiec/devops/projects/personal/mason-learn/web-app-1/target/web-app-1-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/spring-boot-3.1.3.jar!/
[0.631s][info][class,load] org.springframework.boot.logging.java.JavaLoggingSystem source: jar:file:/C:/Users/jiec/devops/projects/personal/mason-learn/web-app-1/target/web-app-1-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/spring-boot-3.1.3.jar!/
[0.632s][info][class,load] java.util.logging.LogManager source: shared objects file
[0.633s][info][class,load] jdk.proxy2.$Proxy2 source: __dynamic_proxy__
[0.634s][info][class,load] org.springframework.core.annotation.AttributeMethods source: jar:file:/C:/Users/jiec/devops/projects/personal/mason-learn/web-app-1/target/web-app-1-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/spring-core-6.0.11.jar!/
[0.635s][info][class,load] java.lang.invoke.LambdaForm$DMH/0x0000021dae094000 source: __JVM_LookupDefineClass__
[0.635s][info][class,load] org.springframework.core.annotation.AttributeMethods$$Lambda$63/0x0000021dae092b18 source: org.springframework.core.annotation.AttributeMethods
[0.636s][info][class,load] org.springframework.core.annotation.AttributeMethods$$Lambda$64/0x0000021dae092db0 source: org.springframework.core.annotation.AttributeMethods
[0.637s][info][class,load] org.springframework.core.annotation.RepeatableContainers$StandardRepeatableContainers$$Lambda$65/0x0000021dae092ff0 source: org.springframework.core.annotation.RepeatableContainers
[0.637s][info][class,load] org.springframework.core.annotation.AnnotationTypeMappings source: jar:file:/C:/Users/jiec/devops/projects/personal/mason-learn/web-app-1/target/web-app-1-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/spring-core-6.0.11.jar!/
[0.638s][info][class,load] org.springframework.core.annotation.IntrospectionFailureLogger source: jar:file:/C:/Users/jiec/devops/projects/personal/mason-learn/web-app-1/target/web-app-1-0.0.1-SNAPSHOT.jar!/BOOT-INF/lib/spring-core-6.0.11.jar!/
```

## 一个war包的基本结构
```
META-INF/
    MANIFEST.MF
WEB-INF/
    web.xml
    jsp/
        helloWorld.jsp
    classes/
        static/
        templates/
        application.properties
    lib/
        // *.jar files as libs
```


## Tomcat 加载class的顺序
> It's all described in Tomcat's ClassLoading HOW-TO. It's not necessarily in alphabetic order. If you observed that behaviour, it should absolutely not be relied upon if you intend to keep your webapp portable across servers. For example, Tomcat 6 "coincidentally" orders it, but Tomcat 8 does not.
> Summarized, the loading order is as follows:
> - bootstrap/system (JRE/lib, then server.loader)
> - webapp libraries (WEB-INF/classes, then WEB-INF/lib)
> - common libraries (common.loader, then Tomcat/lib)
> - webapp-shared libraries (shared.loader)
> 
>If you would like to guarantee that JAR X is loaded after JAR Y, then you'd need to put JAR X in one of the places which appears later in the listing above.
>There are exceptions however, which are mentioned in the tomcat docs
>Lastly, the web application class loader will always delegate first for JavaEE API classes for the specifications implemented by Tomcat (Servlet, JSP, EL, WebSocket). All other class loaders in Tomcat follow the usual delegation pattern.
>That means if a webapp contains any JavaEE classes (javax.*), then it will be ignored by tomcat.
>For each loader, the classes are just loaded by the JVM in the order whenever they needs to be imported/executed and are not loaded yet.

## 在k8s容器中
一个java类到底从哪个jar包里加载出来，取决于container底层VM的文件系统（甚至某个版本）


## Java项目的依赖冲突
用`xwork-core` 和 `struts2-core`作为例子。这两个包是相互冲突的，因为含有一些package名字，class名字完全相同的类/接口。但是这些类/接口不是兼容的，有可能A在`struts2-core`里面是个接口，但是在`xwork-core`里面是个具体的类。那么应用起来的时候，就和这个类的加载顺序很有关系了， 顺序错了，可能就起不来。

同时，一个项目里面，就不应该用这种相互冲突的依赖，基本是一个替换的关系，得要彻底替换掉。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.amason</groupId>
    <artifactId>mason-test-pom</artifactId>
    <version>1.0</version>
    <name>mason-test-pom</name>
    <description>mason-test-pom</description>
    <properties>
        <java.version>11</java.version>
    </properties>

    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.apache.struts.xwork/xwork-core -->
        <dependency>
            <groupId>org.apache.struts.xwork</groupId>
            <artifactId>xwork-core</artifactId>
            <version>2.3.37</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.apache.struts/struts2-core -->
        <dependency>
            <groupId>org.apache.struts</groupId>
            <artifactId>struts2-core</artifactId>
            <version>2.5.30</version>
        </dependency>
    </dependencies>

</project>
```

## 链接
- [Order of loading jar files from lib directory](https://stackoverflow.com/questions/5474765/order-of-loading-jar-files-from-lib-directory)



