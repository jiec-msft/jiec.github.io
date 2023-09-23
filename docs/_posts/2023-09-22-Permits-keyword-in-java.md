---
layout: post
title:  "The permits keyword in Java"
date:   2023-09-22 17:08:00 +0800
categories: Java
---


In Java, the permits keyword is used in conjunction with sealed classes1. Sealed classes are a new feature introduced in Java 171. They allow you to control which classes can implement or extend a sealed class or interface1.

Here’s an example of how to use the permits keyword with a sealed interface:
```java
public sealed interface Service permits Car, Truck {
  int getMaxServiceIntervalInMonths();
  default int getMaxDistanceBetweenServicesInKilometers() {
    return 100000;
  }
}
```

In this example, the Service interface is sealed and only permits the `Car` and `Truck` classes to implement `it1`.