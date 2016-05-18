---
layout: post_layout
title:  "利用IntelliJ IDEA 15建立Spring项目"
date:   2015-11-12 22:40PM
categories: jekyll update
type: 2
summary: "本文将对如何利用IntelliJ 15搭建一个Hello Wolrd项目进行简单介绍,不用担心自己没有Spring的基础,本文主要目的只是为了让学Spring的新手能够快速写出Spring项目,如果想更深入的了解,可以去看Spring的指导手册http://spring.io/guides。"
icon: "java-icon.jpg"
---
#### 开篇

>本文将对如何利用IntelliJ 15搭建一个Hello Wolrd项目进行简单介绍, 不用担心自己没有Spring的基础,主要目的是只是为了让学Spring的新手能够快速写出Spring项目,如果想
更深入的了解,可以去看Spring的指导手册http://spring.io/guides.

#### 1.建立新项目

Create Project

![](/../img/build-spring/1.png)

Select Spring Initializr and SDK

![](/../img/build-spring/2.png)


Next

![](/../img/build-spring/3.png)


Choose Your Dependency

![](/../img/build-spring/4.png)


Finish

![](/../img/build-spring/5.png)

#### 2.项目配置

打开pom.xml文件,如下:

{% highlight xml %}

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.example</groupId>
	<artifactId>demo</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>demo</name>
	<description>Demo project for Spring Boot</description>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.2.7.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.hibernate</groupId>
			<artifactId>hibernate-validator</artifactId>
			<version>5.1.3.Final</version>
		</dependency>
		<dependency>
			<groupId>javax.el</groupId>
			<artifactId>javax.el-api</artifactId>
			<version>2.2.4</version>
		</dependency>
		<dependency>
			<groupId>org.glassfish.web</groupId>
			<artifactId>javax.el</artifactId>
			<version>2.2.4</version>
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
{% endhighlight %}

注意:在依赖项中,默认的依赖是不会加入Validation的依赖的,必须得手动加入:

{% highlight xml %}
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>5.1.3.Final</version>
</dependency>
<dependency>
    <groupId>javax.el</groupId>
    <artifactId>javax.el-api</artifactId>
    <version>2.2.4</version>
</dependency>
<dependency>
    <groupId>org.glassfish.web</groupId>
    <artifactId>javax.el</artifactId>
    <version>2.2.4</version>
</dependency>
{% endhighlight %}

#### 3.添加Controller

IDEA已经默认为我们得程序生成了入口:

{% highlight java %}

package com.example;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}


{% endhighlight %}

我们只需要手动加入WidgetController:

{% highlight java %}

package com.example;

import org.springframework.http.MediaType;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

/**
 * Created by twer on 11/12/15.
 */
@RequestMapping("/api/**")
@RestController
public class WidgetController {
    @RequestMapping(method = RequestMethod.GET, produces = {MediaType.APPLICATION_JSON_VALUE})
    public String sayHello() {
        return "hello, you've successed..........";
    }
}

{% endhighlight %}

#### 4.运行

goto:   http://localhost:8080/api

output: hello, you've successed..........


`注：此文为译文`[原文请戳](http://patrickgrimard.com/2014/08/14/how-to-build-a-spring-boot-application-using-intellij-idea/)

