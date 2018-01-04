---
layout: post
title: "Spring Boot快速入门"
description: "来和我一起学习使用Spring Boot,一起构建简单快捷的Spring应用！"
date: 2018-1-4
tags: [Spring Boot, Java, Web, Maven]
comments: true
share: true
---

## 简介

`Spring Boot`让`Spring`应用变的更轻量化. 让我们可以仅靠一个`Java`类来运行一个`Spring`应用,也可以打包应用为jar包后通过`java -jar`命令来运行这个`Spring Web`应用.

`Spring Boot`的主要优点:
- 为所有`Spring`开发者更快的入门
- 开箱即用,提供各种默认配置来简化项目配置
- 内嵌式容器简化Web项目
- 没有冗余代码生成和对XML配置的要求

## 快速入门

此处要求完成`Spring Boot`基础项目的搭建,并且实现一个简单的`HTTP`请求处理,通过这个例子对`Spring Boot`有一个处理的了解,并体验其结构简单,开发快速的特性.

此处采用`java1.8.0_121`、`Spring Boot1.5.9`进行调试

## 构建项目

此处采用两种方式来构建项目

### 使用Maven构建项目

1. 访问 [SPRING INITIALIZR](http://start.spring.io/)

2. 选择构建工具为`Maven Project`,语言为`Java`,`Spring Boot`的版本选择为`1.5.9`,其余信息可参考下图

 ![](http://oih7sazbd.bkt.clouddn.com/FkOblv1cPBb6a-_DeKTJKTt0Z375)

3. 点击绿色按钮`Generate Project`下载生成的项目的压缩包.

4. 以 [IDEA](https://www.jetbrains.com/idea/) 为例,我们需要导入解压后的项目压缩包 :
> 菜单中按照如下顺序操作: File -> New -> Project from Existing Sources -> 选择解压后的压缩包
> 在弹出的第一个框内选择 import project form external model,并在下方选择maven,然后一路next直到最后一个页面点击finish.

### 使用IDEA自带的Spring Initializr来构建Spring Boot工程

1. 打开IDEA,菜单中按照以下顺序操作: `File -> New -> Project`,打开如下图页面,可以看到`Choose Initializr Service URL`的默认路径还是`Spring`官方提供的`Spring Initializr`工具的地址,所以在这里创建的工程实际上也是基于它的`Web`工具来实现的.
 ![](http://oih7sazbd.bkt.clouddn.com/Fpx7nrzohBmfr5DaJJ6iNtZwkmzt)

2. 直接点击`Next`进入下一步,如下图所示,在这里可以填写我们的工程信息以及选择我们的工程类型,`Type`可选为`Maven`或`Gradle`,`Language`可选为`Java`、`Kotlin`等.
 ![](http://oih7sazbd.bkt.clouddn.com/Fo_m6kMUvvj4mxeYvEEILAoy5J4E)

3. 再次点击`Next`进入下一步,如下图所示,在这里可以选择`Spring Boot`版本和依赖管理,同时,我们还可以发现,这个窗口不仅包含了`Spring Boot`的各种依赖,还包含了`Spring Cloud`的过日子依赖.
 ![](http://oih7sazbd.bkt.clouddn.com/Fu8ehbXRVp729grkk6MVWCg0j5Vv)

4. 最后再次点击`Next`,会进入到我们创建工程的最后一个页面,如下图所示,填写我们的项目名称以及选择我们的项目路径,结束填写后点击`Finish`后就可以完成这次使用`IDEA`构建`Spring Boot`的工程.
 ![](http://oih7sazbd.bkt.clouddn.com/FiPLwX9HaD1sEW0jKqH0lteEzdRw)


**IDEA的Spring Initializr虽然还是基于官方实现,但是通过工具来进行调用并直接将结果构建到我们本地,这个构建流程变的更加顺畅.**

## 项目结构解析

![](http://oih7sazbd.bkt.clouddn.com/FhxxhQyfi3cISdwY-4C4kZSHQEex)

之前的步骤,已经使我们完成了项目的基本构建. 如上图所示,`Spring Boot`的基础结构供三个文件:

- `Chapter1Application`     : 位于`src/main/java`下的程序入口
- `application.properties`  : 位于`src/main/resources`下的配置文件
- `Chapter1ApplicationTests`: 位于`src/test`下的测试入口

由生成程序生成的`Chapter1Application`和`Chapter1ApplicationTests`类都可以直接运行来启动当前创建的项目. 由于当前项目没有配备任何数据访问或`Web`模块,所以程序会在加载完`Spring`之后结束运行.

## 引入Web模块

打开项目的`pom.xml`,会发现`dependencies`下只引入了两个模块:

- `spring-boot-starter` : 核心模块,包括自动配置支持、日志和`YAML`
- `spring-boot-starter-test` : 测试模块，包括`JUnit`、`Hamcrest`、`Mockito`

```xml
<dependencies>
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter</artifactId>
	</dependency>
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-test</artifactId>
		<scope>test</scope>
	</dependency>
</dependencies>
```

我们引入Web模块,需要在`dependencies`中追加`spring-boot-starter-web`模块

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

完成后保存,并点击右下角弹窗中的`import Changes`对`pom.xml`中库的更改进行更新即可.

## 编写Hello World

在`src/main/java`下创建`package`为`com.chapter1.controller`,在`package`下创建`HelloController`类,内容如下图所示 :

![](http://oih7sazbd.bkt.clouddn.com/Fl1Q3avi_mqJZXikKxjfPeFkW3nj)

启动主程序`Chapter1Application`,启动成功后打开`http://localhost:8080/hello`,可以看到页面成功输出了`Hello World`.

## 编写单元测试

打开`src/test`下的测试入口`Chapter1ApplicationTests`类,编写一个简单的单元测试来模拟`HTTP`请求,如下图所示 :

![](http://oih7sazbd.bkt.clouddn.com/FsQ5JOGzHQxudE1LJT5aqXZ2l2Uu)

首先我们使用`MockServletContext`来构建一个空的`WebApplicationContext`,这样我们创建的`HelloController`就可以在`@Before`函数中创建并传递到`MockMvcBuilders.standaloneSetup()`函数中.

`status()`与`content()`函数报错是因为需要引入以下内容 :

```java
import static org.hamcrest.Matchers.equalTo;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;
```
完成上述操作后,可以启动`@Test`标注的测试方法,测试成功后即完成了此次目标: 通过`Maven`构建一个空白的`Spring Boot`项目,然后通过引用`Web`模块实现了一个简单的`HTTP`请求处理.
