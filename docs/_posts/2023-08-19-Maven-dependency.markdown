# Maven 依赖管理
## 依赖的类型
1. 直接依赖
   - ```
     <dependency>
       <groupId>junit</groupId>
       <artifactId>junit</artifactId>
       <version>4.12</version>
     </dependency>
     ```
2. 传递依赖 （个人觉得叫间接依赖也可以）
   - 就是直接依赖的依赖
