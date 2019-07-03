# Web Server 应用

## 切换 Web Server

### 切换其他Servlet容器

* Tomcat -&gt; Jetty

```markup
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-web</artifactId>
	<exclusions>
		<exclusion>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-tomcat</artifactId>
		</exclusion>
	</exclusions>
</dependency>

<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-jetty</artifactId>
</dependency>
```

### 替换Servlet容器

* WebFlux 

```markup
<!--当与传统Servlet容器共存时传统Servlet容器当优先级更高-->>
<!--<dependency>-->
	<!--<groupId>org.springframework.boot</groupId>-->
	<!--<artifactId>spring-boot-starter-web</artifactId>-->
	<!--<exclusions>-->
		<!--<exclusion>-->
			<!--<groupId>org.springframework.boot</groupId>-->
			<!--<artifactId>spring-boot-starter-tomcat</artifactId>-->
		<!--</exclusion>-->
	<!--</exclusions>-->
<!--</dependency>-->
<!--<dependency>-->
	<!--<groupId>org.springframework.boot</groupId>-->
	<!--<artifactId>spring-boot-starter-jetty</artifactId>-->
<!--</dependency>-->

<dependency>
    <groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-webflux</artifactId>
</dependency>
```

## 自定义Servlet Web Server

* `WebServerFactoryCustomizer`

## 自定义Reactive Web Server

* `ReactiveWebServerFactoryCustomizer`

