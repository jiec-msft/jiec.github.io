---
layout: post
title: SearchStrategy.CURRENT
date: 2023-09-27 23:22:00 +0800
categories: Spring
---
`SearchStrategy.CURRENT` 效果是什么？ 例如：

```java
@Configuration(proxyBeanMethods = false)  
@ConditionalOnClass(name = "jakarta.ws.rs.client.ClientRequestFilter")  
@ConditionalOnBean(value = AbstractDiscoveryClientOptionalArgs.class, search = SearchStrategy.CURRENT)  
static class DiscoveryClientOptionalArgsTlsConfiguration {  
  
DiscoveryClientOptionalArgsTlsConfiguration(TlsProperties tlsProperties,  
AbstractDiscoveryClientOptionalArgs optionalArgs) throws GeneralSecurityException, IOException {  
logger.info("Eureka HTTP Client uses Jersey");  
setupTLS(optionalArgs, tlsProperties);  
}  
  
}
```
