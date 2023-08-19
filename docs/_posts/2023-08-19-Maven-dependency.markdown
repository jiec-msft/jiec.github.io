---
layout: post
title:  "Maven dependency scopes"
date:   2023-08-19 16:21:08 +0800
categories: Maven
---

# Maven 依赖管理
## 依赖的类型
1. 直接依赖
   - ```xml
     <dependency>
       <groupId>junit</groupId>
       <artifactId>junit</artifactId>
       <version>4.12</version>
     </dependency>
     ```
2. 传递依赖 （个人觉得叫间接依赖也可以）
   - 就是直接依赖的依赖

## 依赖的Scope
这是Maven做依赖管理的时候比较有意思，同时设计得比较巧妙的一个地方。Scop可以用来限制依赖的传递性。同时，scope不同，那么在不同的`build task`中就会把classpath改掉。一共有6种依赖:

1. Compile
   - 这是默认的scope
   - 有这种scope的依赖，会被加载到各种build task的classpath里面去。
   - 同时，这个会把这个scope加到它的上游依赖。
   - 而且，这个scope的依赖具有传递性。假设B依赖A的scope是compile，C依赖B的scope是compile，那么C依赖A的scope也是compile。
   - ```xml
     <dependency>
       <groupId>commons-lang</groupId>
       <artifactId>commons-lang</artifactId>
       <version>2.6</version>
     </dependency>
     ```
2. Provided
   - 用来标记某种依赖不需要带到runtime, runtime提供的。 
   - 例如Lombok，只是编译的时候需要用一下，用来生成代码的，runtime根本不需要这个依赖。
   - 又例如当我们要部署一个web application到一个container中，这个container可能已经包含了一些依赖包。例如Servlet API.
     - ```xml
       <dependency>
           <groupId>javax.servlet</groupId>
           <artifactId>javax.servlet-api</artifactId>
           <version>4.0.1</version>
           <scope>provided</scope>
       </dependency>
       ```
    - 这种依赖在编译/测试阶段都会加载到classpath里。
3. Runtime
   - 用来标记这个依赖在编译/测试阶段都用不到，只在运行的时候需要用一下。
   - 例如JDBC driver
   - ```xml
     <dependency>
         <groupId>mysql</groupId>
         <artifactId>mysql-connector-java</artifactId>
         <version>8.0.28</version>
         <scope>runtime</scope>
     </dependency>
     ```
4. Test
   - 只给Test task用，写到这个classpath里面
5. System
   - 已经**Deprecated**
   - 和Provided很像，但是要求指定一个路劲
   - ```xml
     <dependency>
         <groupId>com.baeldung</groupId>
         <artifactId>custom-dependency</artifactId>
         <version>1.3.2</version>
         <scope>system</scope>
         <systemPath>${project.basedir}/libs/custom-dependency-1.3.2.jar</systemPath>
     </dependency>
     ```
6. Import
   - 仅用于type=pom
   - 顾名思义，就是把这个pom文件全部给加来进来，可以用复制-粘贴来理解。
   - ```xml
     <dependency>
         <groupId>com.baeldung</groupId>
         <artifactId>custom-project</artifactId>
         <version>1.3.2</version>
         <type>pom</type>
         <scope>import</scope>
     </dependency>
     ```


## scope的传递性
- `provided` and `test` 不会被包括在main project中。大白话就是在最终的jar里面，不会包含进去。
- For the compile scope, all dependencies with runtime scope will be pulled in with the runtime scope in the project, and all dependencies with the compile scope will be pulled in with the compile scope in the project.
- For the provided scope, both runtime and compile scope dependencies will be pulled in with the provided scope in the project.
- For the test scope, both runtime and compile scope transitive dependencies will be pulled in with the test scope in the project.
- For the runtime scope, both runtime and compile scope transitive dependencies will be pulled in with the runtime scope in the project.