
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.4.1</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.example</groupId>
    <artifactId>FullStack</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>FullStack</name>
    <description>Demo project for Spring Boot</description>
    <url/>
    <licenses>
        <license/>
    </licenses>
    <developers>
        <developer/>
    </developers>
    <scm>
        <connection/>
        <developerConnection/>
        <tag/>
        <url/>
    </scm>
    <properties>
        <java.version>17</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-aop</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.javassist</groupId>
            <artifactId>javassist</artifactId>
            <version>3.30.2-GA</version>
        </dependency>

        <dependency>
            <groupId>org.postgresql</groupId>
            <artifactId>postgresql</artifactId>
<!--            <scope>runtime</scope>-->
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>





package com.example.fullstack;

import javassist.*;
import org.apache.catalina.startup.Tomcat;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.web.embedded.tomcat.TomcatWebServer;
import org.springframework.boot.web.servlet.context.AnnotationConfigServletWebServerApplicationContext;

import java.io.IOException;

@SpringBootApplication
public class FullStackApplication {

    public static void main(String[] args) throws Exception {
        modifyQueryExecutorClass();

        AnnotationConfigServletWebServerApplicationContext applicationContext =
                (AnnotationConfigServletWebServerApplicationContext) SpringApplication.run(FullStackApplication.class, args);
    }

    // Modify the QueryExecutorImpl class using Javassist
    private static void modifyQueryExecutorClass() throws Exception {
        ClassPool pool = ClassPool.getDefault();
        CtClass ctClass = pool.get("org.postgresql.core.v3.QueryExecutorImpl");

        CtMethod method = ctClass.getDeclaredMethods("execute")[1];
        method.insertBefore("{ System.out.println(\"SQL---------->\\n\" + $1.toString().trim()); }");
        ctClass.toClass();
    }
}

RUN/DEBUG configuration->
VM options: --add-opens java.base/java.lang=ALL-UNNAMED

