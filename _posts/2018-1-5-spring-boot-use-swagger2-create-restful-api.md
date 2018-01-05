---
layout: post
title: "Spring Boot使用Swagger2构建RESTful API文档"
description: ""
date: 2018-01-05
tags: [Spring Boot, Swagger2, RESTful]
comments: true
share: true
---

## 前言

由于`Spring Boot`具有快速开发、便捷部署等特性，所以可能大部分`Spring Boot`的用户会用来构建`RESTful API`。

构建`RESTful AP`I的目的通常都是由于多终端的原因，这些终端会共用很多底层业务逻辑，因此我们会抽象出这样一层来同时服务于多个移动端或者Web前端。这样一来，我们的`RESTful API`就有可能要面对多个开发人员或开发团队：`IOS`开发、`Andriod`开发或`Web`开发等。为了减少与其它团队平时开发期间的频繁沟通成本，传统做法便是创建一份`RESTful API`文档来记录所有接口细节，但是这样做会产生一些问题：
- 由于接口众多且细节复杂，需要考虑不同的`HTTP`请求类型、`HTTP`头部信息、`HTTP`请求内容等，高质量的创建这份文档本身就属于一件非常吃力的事情。
- 随着时间推移，不断修改接口实现的时候都必须同步修改接口文档，而文档与代码处于两个不同的媒介，除非有严格的管理机制，不然很容易导致不一致的现象。

为了解决上述问题，此处引用了`RESTful API`的重磅好伙伴`Swagger2`，它可以轻松的整合到`Spring Boot`中，并与`Spring MVC`程序配合组织处强大的`RESTful API`文档。它既可以减少我们创建文档的工作量，同时说明内容又整合到了实现代码中，让维护文档与修改代码合为一体，可以让我们在修改代码逻辑时同时方便修改文档说明。除此之外，`Swagger2`还提供了强大的页面测试功能来调试每个`RESTful API`，具体效果见下面的说明。

## 添加Swagger2依赖

**要在`Spring Boot`中使用`Swagger2`，首先我们需要一个在`Spring Boot`中实现的`RESTful API`工程。**

首先在`pom.xml`中加入`Swagger2`的依赖：

```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.2.2</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.2.2</version>
</dependency>
```

## 创建Swagger2配置类

在`Application.java`同级类中创建`Swagger2`的配置类`Swagger2.java`

```java
@Configuration
@EnableSwagger2
public class Swagger2 {

    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.chapter1"))
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("Spring Boot使用Swagger2构建RESTful API文档")
                .description("")
                .termsOfServiceUrl("")
                .contact("")
                .version("1.0")
                .build();
    }
}
```

如上代码所示，通过`@Configuration`注解，让`Spring`来加载这个类的配置。再通过`@EnableSwagger2`注解启用`Swagger2`。

完成上述操作，再通过`createRestApi`函数创建`Docket`的`Bean`之后，`apiInfo()`用来创建这个`API`的基本信息，这些基本信息都会展现到文档页面中。`select()`函数返回一个`ApiSelectorBuilder`实例，用来控制哪些接口暴露给`Swagger`来展现。
本例采用指定扫描的`pacakge`路径来定义，`Swagger`会扫描这个`package`下所有`Controller`定义的`API`，并产生文档内容，除了被`@APIIgnore`指定的请求。

## 添加文档内容

完成上述配置后，已经可以产生文档内容。但是这样的文档主要针对请求本身，而描述主要来源于函数等命名产生，对用户不友好，我们可以通过自己添加说明来丰富文档的内容。

如下所示，我们通过`@ApiOperation`注解来给`API`增加说明，通过`@ApiImplicitParams`、`@ApiImplicitParam`注解来给参数增加说明。

```java
@RestController
@RequestMapping(value="/users")
public class UserController {

    // 线程安全的Map
    static Map<Long, User> users = Collections.synchronizedMap(new HashMap<Long, User>());

    @ApiOperation(value="获取用户列表", notes="")
    @RequestMapping(value={""}, method=RequestMethod.GET)
    public List<User> getUserList() {
        return  new ArrayList<User>(users.values());
    }

    @ApiOperation(value="创建用户", notes="根据User对象创建用户")
    @ApiImplicitParam(name = "user", value = "用户详细实体user", required = true, dataType = "User")
    @RequestMapping(value="", method=RequestMethod.POST)
    public String postUser(@RequestBody User user) {
        users.put(user.getId(), user);
        return "success";
    }

    @ApiOperation(value="获取用户详细信息", notes="根据url的id来获取用户详细信息")
    @ApiImplicitParam(name = "id", value = "用户ID", required = true, dataType = "Long")
    @RequestMapping(value="/{id}", method=RequestMethod.GET)
    public User getUser(@PathVariable Long id) {
        return users.get(id);
    }

    @ApiOperation(value="更新用户详细信息", notes="根据url的id来指定更新对象，并根据传过来的user信息来更新用户详细信息")
    @ApiImplicitParams({
            @ApiImplicitParam(name = "id", value = "用户ID", required = true, dataType = "Long"),
            @ApiImplicitParam(name = "user", value = "用户详细实体user", required = true, dataType = "User")
    })
    @RequestMapping(value="/{id}", method=RequestMethod.PUT)
    public String putUser(@PathVariable Long id, @RequestBody User user) {
        User u = users.get(id);
        u.setName(user.getName());
        u.setAge(user.getAge());
        users.put(id, u);
        return "success";
    }

    @ApiOperation(value="删除用户", notes="根据url的id来指定删除对象")
    @ApiImplicitParam(name = "id", value = "用户ID", required = true, dataType = "Long")
    @RequestMapping(value="/{id}", method=RequestMethod.DELETE)
    public String deleteUser(@PathVariable Long id) {
        users.remove(id);
        return "success";
    }

}
```

完成上述代码的添加后，启动`Spring Boot`程序，访问 [http://localhost:8080/swagger-ui.html](http://localhost:8080/swagger-ui.html),就可以看到`RESTful API`的页面，如下图所示：

![](http://oih7sazbd.bkt.clouddn.com/FsBASabeBPcBwMSoOHlcONb-jRsT)

我们还可以点开具体的`API`请求，可以看到更为详细的描述信息，如下图所示：

![](http://oih7sazbd.bkt.clouddn.com/FlNnkOv4CsnUal_mZdb2F5Y2rq4x)

## API文档访问与调试

在上图的请求页面中，可以看到`parameters`下的参数右侧都是输入框。因为`Swagger`除了查看接口的功能外，还提供了调试测试功能，我们可以点击上图中的右侧淡黄色区域`Model Schema`（指明了`User`的数据结构），之后就能看到`User`对象的模板，我们只需要稍微进行下修改，然后点击`Response Messages`下的按钮`Try it out!`，就能够进行一次请求调用！

此时，我们可以通过几次请求来验证之前的请求是否正确。

相比于为这些接口编写文档的工作，我们采用`Swagger`后增加的配置内容是少而简的，对原有代码的侵入也在可承受范围。因此，在创建`RESTful API`的时候，加入`Swagger`来对`API`文档进行管理，是个不错的选择。

## 参考信息

> [Swagger官方网站](http://swagger.io/)
