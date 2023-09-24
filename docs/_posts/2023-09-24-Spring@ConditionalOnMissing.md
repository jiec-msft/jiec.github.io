---
layout: post
title:  "ConditionalOnMissing annotation"
date:   2023-09-24 15:49:00 +0800
categories: Spring
---


[The `@ConditionalOnMissingBean` annotation in Spring is used to **load a bean only if a given bean is missing**](https://stackoverflow.com/questions/50796810/what-does-the-spring-annotation-conditionalonmissingbean-do)[1](https://stackoverflow.com/questions/50796810/what-does-the-spring-annotation-conditionalonmissingbean-do)[2](https://codeahoy.com/questions/java/10/). [It allows you to conditionally register a bean based on the absence of another bean in the application context](https://stackoverflow.com/questions/50796810/what-does-the-spring-annotation-conditionalonmissingbean-do)[1](https://stackoverflow.com/questions/50796810/what-does-the-spring-annotation-conditionalonmissingbean-do)[2](https://codeahoy.com/questions/java/10/).

Here’s an example of how it can be used:

```java
@Bean
@ConditionalOnMissingBean(SomeBean.class)
public SomeBean otherBean() {
    return new SomeBean();
}
```

Copy

[In the above code, the `otherBean()` method will be invoked and the resulting bean will be registered only if there is no other bean of type `SomeBean` present in the context](https://stackoverflow.com/questions/50796810/what-does-the-spring-annotation-conditionalonmissingbean-do)[1](https://stackoverflow.com/questions/50796810/what-does-the-spring-annotation-conditionalonmissingbean-do). [If a bean of type `SomeBean` is already present, the `otherBean()` method will not be invoked](https://stackoverflow.com/questions/50796810/what-does-the-spring-annotation-conditionalonmissingbean-do)[1](https://stackoverflow.com/questions/50796810/what-does-the-spring-annotation-conditionalonmissingbean-do).

[The `@ConditionalOnMissingBean` annotation is useful in scenarios where you want to provide a **default or fallback bean** in the absence of a particular bean](https://codeahoy.com/questions/java/10/)[2](https://codeahoy.com/questions/java/10/). [For example, you can use it to specify a fallback implementation or a default configuration that gets loaded only if a specific bean is not already defined](https://codeahoy.com/questions/java/10/)[2](https://codeahoy.com/questions/java/10/).

By using conditional annotations like `@ConditionalOnMissingBean`, you can create more flexible and customizable configurations based on the presence or absence of beans in your application context.