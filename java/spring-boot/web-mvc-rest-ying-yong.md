---
description: 'https://github.com/JasperJiang/SpringBoot_Learning/tree/master/springbootrest'
---

# Web MVC REST 应用

## REST 简介

### 基本概念

REST = RESTful = Representational State Transfer

### 架构约束

* 统一接口（Uniform interface）
* C/S架构（Client-Server）
* 无状态（Stateless）
* 可缓存（Cacheable）
* 分层系统（Layered System）
* 按需代码（Code on demand）（可选）

### 统一接口

* 资源识别（Identification of resources）
  * URI（Uniform Resource Identifier）
* 资源操作（Manipulation of resources through representations）
  * HTTP verbs: GET PUT POST PATCH DELETE
* 自描述消息（Self-descriptive messages）
  * Content-Type
  * MIME-Type
  * Media Type: application/javascript  text/html
* 超媒体（HATEOAS）
  * Hypermedia As The Engine Of Application State

## Web MVC REST 支持

### 注解驱动

* 定义：
  * `@Controller`   
  * `@RestController = @Controller + @ResponseBody`
* 映射：
  * `@RequestMapping`   
  * `@*Mapping`
* 请求：
  * `@RequestParam`    
  * `@RequestHeader`   
  * `@CookieValue`
* 响应：
  * `@ResponseBody`    
  * `ResponseEntity`
* 拦截：
  * `@RestControllerAdvice` 
  * `HandlerInterceptor`
* 跨域：
  * `@CrossOrigin`   
  * `CorsFilter`   
  * `WebMvcConfigurer#addCorsMappings`

## REST 内容协商

### 核心组件

* 内容协商管理器：ContentNegotiationManager
* 媒体类型：MediaType
* 消费媒体类型：@RequestMapping\#consumes
* 生产媒体类型：@RequestMapping\#produces
* HTTP消息转换器：HttpMessageConverter
* REST配置器：WebMvcConfigurer
* 处理方法参数解析器：HandlerMethodArgumentResolver
* 处理方法返回值解析器：HandlerMethodReturnValueHandler

### Spring Web MVC REST 处理流程

![](../../.gitbook/assets/image%20%2829%29.png)

### Spring Web MVC REST 内容协商处理流程

![](../../.gitbook/assets/image%20%2817%29.png)

### 理解可请求的媒体类型

经过`ContentNegotiationManager`的`ContentNegotiationStrategy`解析请求中的媒体类型，比如：`Accept`亲求头

* 如果成功解析，返回合法`MediaType`列表
* 否则，返回单元素`*/*`媒体类型列表 - `MediaType.ALL`

### 理解可生成的媒体类型

返回`@Controller` `HandlerMethod` `@RequestMapping.produces()` 属性所指定的`MediaType`列表：

* 如果`@RequestMapping.produces()`存在，返回指定`MediaType`列表
* 否则，返回已注册的`HttpMessageConverter`列表中支持的`MediaType`列表

### 理解`@RequestMapping#consumes`

用于`@Controller` `HandlerMethod` 匹配：

* 如果请求头`Content-Type`媒体类型兼容`@RequestMapping.consumes()`属性，执行该`HandlerMethod`
* 否则`HandlerMethod`不会被调用

### 理解`@RequestMapping#produces`

用于获取可生产的`MediaType`列表

* 如果该列表与请求的媒体类型兼容，执行第一个兼容`HttpMessageConverter`的实现，默认`@RequestMapping#produces`内容到响应头`Content-Type`
* 否则，抛出`HttpMediaTypeNotAcceptableException`, HTTP Status Code:415

### 扩展REST内容协商

#### 需求

实现`Content-Type`为`text/properties`媒体类型到`HttpMessageConverter`

#### 实现步骤

* 实现`HttpMessageConverter` - `PropertiesHttpMessageConverter`
* 配置`PropertiesMessageConverter`到`WebMvcConfigurer#extendMessageConverters`

