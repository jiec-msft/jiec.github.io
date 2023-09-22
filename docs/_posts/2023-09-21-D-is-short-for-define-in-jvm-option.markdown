---
layout: post
title:  "D is short for define in jvm option"
date:   2023-09-21 16:31:00 +0800
categories: Java
---
E.g.
```
java -Dkey=value
```

Here, `D` is short for "define". It is used to define system properties when starting a Java application.

When you set JVM options using the -D flag, you can pass key-value pairs to the Java Virtual Machine (JVM) to configure various aspects of your application. For example, you can use -Dmy.property=value to define a system property named my.property with the value value.

These system properties can be accessed within your Java code using the System.getProperty("my.property") method. This allows you to pass configuration values to your application at runtime without modifying the source code.