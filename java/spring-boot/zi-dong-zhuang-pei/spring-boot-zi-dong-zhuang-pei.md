# Spring Boot 自动装配

### 底层装配技术

* Spring 模式注解装配
* Spring @Enable 模块装配
* Spring 条件装配
* Spring 工厂加载机制
  * 实现类：`SpringFactoriesLoader`
  * 配置资源： `META-INF/spring.factories`

### 实现方式

1. 激活自动装配 - `@EnableAutoConfiguration`
2. 实现自动装配 - `XXXAutoConfiguration`
3. 配置自动装配实现 - `META-INF/spring.factories`

### 自定义自动装配

```java
@Configuration  // 模块注解装配
@EnableHelloWorld // Spring @Enable 装配
@ConditionalOnSystemProperty(name = "user.name", value = "jack") // 条件装配
public class HelloWorldAutoConfiguration {
}
```

* 条件判断： `user.name==jack`
* 模式注解： `@Configuration`
* @Enable模块： `@EnableHelloWorld` -&gt; `HelloWorldImportSelector` -&gt; `HelloWorldConfiguration` -&gt; `helloWorld`



