---
layout: post
title:  "The @Primary annotation in Spring boot application"
date:   2023-09-23 15:49:00 +0800
categories: Spring
---

In a Spring Boot application, the `@Primary` annotation is used to indicate the primary bean when multiple beans of the same type are present for auto-wiring 1. When multiple beans are eligible for auto-wiring, the `@Primary` annotation helps to determine which bean should be given preference.

For instance, let’s say we have two beans of the same type, Employee, in our application context: `JohnEmployee()` and `TonyEmployee()`. If we try to run the application without any further configuration, Spring will throw a `NoUniqueBeanDefinitionException` because it cannot determine which bean to inject.

To resolve this issue, we can use the `@Primary` annotation to mark one of the beans as primary. For example, we can mark `TonyEmployee()` as primary by adding the `@Primary` annotation to its definition:

```Java
@Configuration
public class Config {
    @Bean
    public Employee JohnEmployee() {
        return new Employee("John");
    }

    @Bean
    @Primary
    public Employee TonyEmployee() {
        return new Employee("Tony");
    }
}
```


Now, when we start the application context and try to get an instance of `Employee`, Spring will inject `TonyEmployee()` preferentially over `JohnEmployee()`.