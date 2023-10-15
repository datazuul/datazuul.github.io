---
layout: post
title: Spring Boot Auto-Reload with IntelliJ
author: Ralf Eichinger
toc: true
---

# Configuration

If you want your Spring Boot project reload automatically on changes in code,
you have to make the following configuration to your project:

1. Add `spring-boot-devtools` to your project's dependencies ("optional" to be not packed into JAR).
 
   pom.xml:
   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-devtools</artifactId>
       <scope>runtime</scope>
       <optional>true</optional>
   </dependency>
   ```

2. Configure project settings in IntelliJ:

   * **File -> Settings -> Build, Execution, Deployment -> Compiler**: Activate *Build project automatically*
   * **File -> Settings -> Advanced Settings**: Activate *Allow auto-make to start even if developed application is currently running*

That's it!
