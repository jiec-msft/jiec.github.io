---
layout: post
title:  "ConfigurationProperties annotation"
date:   2023-09-24 15:49:00 +0800
categories: Spring
---

[The `@ConfigurationProperties` annotation in **Spring Boot** is used to **bind external configuration properties** to a **Java object**](https://mkyong.com/spring-boot/spring-boot-configurationproperties-example/)[1](https://mkyong.com/spring-boot/spring-boot-configurationproperties-example/)[2](https://www.javadevjournal.com/spring-boot/configuration-properties-in-spring-boot/). [It allows you to map or bind the entire external configuration values in `.properties` or `.yml` files to a Java object](https://mkyong.com/spring-boot/spring-boot-configurationproperties-example/)[1](https://mkyong.com/spring-boot/spring-boot-configurationproperties-example/)[2](https://www.javadevjournal.com/spring-boot/configuration-properties-in-spring-boot/).

[By using `@ConfigurationProperties`, you can **centralize your configuration properties** into a single class and access them easily throughout your application](https://mkyong.com/spring-boot/spring-boot-configurationproperties-example/)[1](https://mkyong.com/spring-boot/spring-boot-configurationproperties-example/)[2](https://www.javadevjournal.com/spring-boot/configuration-properties-in-spring-boot/). [This helps in organizing and managing the configuration properties in a more structured way](https://edgeservices.bing.com/edgesvc/chat?udsframed=1&form=SHORUN&clientscopes=chat,noheader,udsedgeshop,channelstable,&shellsig=063a90ae8bb2f707097c3b5ce269d967d4e127ac&setlang=en-US&darkschemeovr=1#)[1](https://mkyong.com/spring-boot/spring-boot-configurationproperties-example/)[2](https://www.javadevjournal.com/spring-boot/configuration-properties-in-spring-boot/).

Here’s an example of how `@ConfigurationProperties` can be used:

```java
@Configuration
@ConfigurationProperties(prefix = "mail")
public class ConfigProperties {
    private String hostName;
    private int port;
    private String from;

    // standard getters and setters
}
```

Copy

[In the above code, the `ConfigProperties` class is annotated with `@ConfigurationProperties` and `@Configuration`](https://edgeservices.bing.com/edgesvc/chat?udsframed=1&form=SHORUN&clientscopes=chat,noheader,udsedgeshop,channelstable,&shellsig=063a90ae8bb2f707097c3b5ce269d967d4e127ac&setlang=en-US&darkschemeovr=1#)[3](https://www.baeldung.com/configuration-properties-in-spring-boot). [The `prefix` attribute specifies the common prefix for all the properties that will be bound to this class](https://edgeservices.bing.com/edgesvc/chat?udsframed=1&form=SHORUN&clientscopes=chat,noheader,udsedgeshop,channelstable,&shellsig=063a90ae8bb2f707097c3b5ce269d967d4e127ac&setlang=en-US&darkschemeovr=1#)[3](https://www.baeldung.com/configuration-properties-in-spring-boot). [For example, if you have properties like `mail.hostName`, `mail.port`, and `mail.from`, they will be automatically bound to the corresponding fields in the `ConfigProperties` class](https://edgeservices.bing.com/edgesvc/chat?udsframed=1&form=SHORUN&clientscopes=chat,noheader,udsedgeshop,channelstable,&shellsig=063a90ae8bb2f707097c3b5ce269d967d4e127ac&setlang=en-US&darkschemeovr=1#)[3](https://www.baeldung.com/configuration-properties-in-spring-boot).

[By using this approach, you can easily access these configuration properties throughout your application by injecting an instance of the `ConfigProperties` class wherever needed](https://www.baeldung.com/configuration-properties-in-spring-boot)[3](https://www.baeldung.com/configuration-properties-in-spring-boot).

[Please note that you need to enable the binding of `@ConfigurationProperties` by adding the `@EnableConfigurationProperties(ConfigProperties.class)` annotation to your main Spring application class](https://www.baeldung.com/configuration-properties-in-spring-boot)[3](https://www.baeldung.com/configuration-properties-in-spring-boot).



