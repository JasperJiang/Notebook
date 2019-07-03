# 传统Servlet应用

## 依赖

```markup
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

## Servlet组件

* Servlet
  * 实现
  * URL映射
  * 注册
* Filter
* Listener

## Servlet注册

### Servlet注解

* `@ServletComponentScan` +
  * `@WebServlet`
  * `@WebFilter`
  * `@WebListener`

### Spring Bean

* `@Bean` +
  * Servlet
  * Filter
  * Listener

### RegistrationBean

* `ServletRegistrationBean`
* `FilterRegistrationBean`
* `ServletListenerRegistrationBean`

## 异步非阻塞

### 异步Servlet

* `javax.servlet.ServletRequest#startAsync()`
* `javax.servlet.AsyncContext`

### 非阻塞Servlet

* `javax.servlet.ServletInputStream#setReadListener`
  * `javax.servlet.ReadListener`
* `javax.servlet.ServletOutputStream#setWriteListener`
  * `javax.servlet.WriteListener`

