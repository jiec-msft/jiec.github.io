---
layout: post
title:  "Enum can be null in java"
date:   2023-09-22 17:08:00 +0800
categories: Java
---

See code: 

```java
package com.mypackage;

import org.junit.Test;

public class TestHttpStatusCode {

    @Test
    public void testHttpStatusCode(){
        MyEnum myEnum = null;
        System.out.println(myEnum == null);
        System.out.println(myEnum);
    }

    public enum MyEnum {
        A,B,C
    }
}

```

Output
```shell
true
null
```