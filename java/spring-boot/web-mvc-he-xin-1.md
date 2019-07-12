# Web MVC 核心

## 理解 Spring Web MVC 构架

## 基础架构：Servlet

### 核心架构：前段控制器（Front Controller）

![](../../.gitbook/assets/image%20%283%29.png)

{% embed url="http://www.corej2eepatterns.com/FrontController.htm" %}

### Spring Web MVC 架构

![](../../.gitbook/assets/image%20%2814%29.png)

## 认识Spring Web MVC

### Spring Framework 时代的一般认识

#### 实现 Controller

```java
@Controller
public class HelloWorldController{
    @RequestMapping("")
    public String index(){
        return "index";
    }
}
```

#### 配置 Web MVC 组件

```markup
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	   					   http://www.springframework.org/schema/beans/spring-beans.xsd
	   					   http://www.springframework.org/schema/context
	   					   https://www.springframework.org/schema/context/spring-context.xsd">

   <context:component-scan base-package="com.jasper.web" />

    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping" />
    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter" />

    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

</beans>
```

#### 部署 DispatcherServlet

```markup
<web-app>
    <servlet>
        <servlet-name>app</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>/WEB-INF/app-context.xml</param-value>
        </init-param>
    </servlet>

    <servlet-mapping>
        <servlet-name>app</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

#### 使用可执行Tomcat Maven 插件

```markup
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
            <version>2.1</version>
            <executions>
                <execution>
                    <id>tomcat-run</id>
                    <goals>
                        <goal>exec-war-only</goal>
                    </goals>
                    <phase>package</phase>
                    <configuration>
                        <!-- ServletContext path -->
                        <path>/</path>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

### Spring Framework 时代的重新认识

#### Web MVC 核心组件

处理器管理

* 映射： `HandlerMapping`
* 适配器：`HandlerAdapter`
* 执行：`HandlerExecutionChain`

页面渲染

* 试图解析：`ViewResolver`
* 国际化：`LocaleResolver`, `LocaleContextResolver`
* 个性化：ThemeResolver

异常处理

* 异常解析：HandlerExceptionResolver

![](../../.gitbook/assets/image%20%2811%29.png)

### Web MVC 注解驱动

版本依赖 Spring Framework 3.1+

* 注解配置：`@Configuration` \(Spring 范式注解\)
* 组件激活：`@EnableWebMvc` \(Spring 模式装配\)
* 自定义组件：`WebMvcConfigurer` \(Spring Bean\)
* 模型属性：`@ModelAttribute`
* 请求头：`@RequestHeader`
* Cookie: `@CookieValue`
* 校验参数：`@Valid`, `@Validated`
* 注解处理：`@ExceptionHandler`
* 切面通知：`@ControllerAdvice`

### Web MVC 自动装配

* Servlet依赖: Servlet 3.0+
* Spring SPI:  ServletContainerInitializer
* Spring 适配：SpringServletContainerInitializer
* Spring SPI: WebApplicationInitializer
* 编程驱动：AbstractDispatcherServletInitializer
* 注解驱动：AbstractAnnotationConfigDispatcherServletInitializer



