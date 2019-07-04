# Spring Framework 手动装配

## Spring 模式/范式注解装配

### 模式注解（Stereotype Annotations）

### 模式注解举例

* `@Repository`
* `@Component`
* `@Service`
* `@Controller`
* `@Configuration`

### 装配方式

#### `<context: component-scan>` 方式

#### `@ComponentScan` 方式

```java
@ComponentScan(basePackages = "com.example.demo.repository")
public class RepositoryBootstrap {
 ...
}
```

### 自定义模式注解

#### `@Component` 派生性

```java
/**
 * 一级 {@link Repository @Repository}
 */

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Repository
public @interface FirstLevelRepository {

    String value() default "";
}
```

* `@Compoent`
  * `@Repository`
    * `@FirstLevelRepository`

#### `@Component` 层次性

```java
/**
 * 二级 {@link FirstLevelRepository @FirstLevelRepository}
 */

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@FirstLevelRepository
public @interface SecondLevelRepository {

    String value() default "";
}
```

* `@Compoent`
  * `@Repository`
    * `@FirstLevelRepository`
      * `@SecondLevelRepository`

## Spring @Enable 模块装配

### 举例

* `@EnableWebMvc`
* ...

### 实现方式

#### 注解驱动方式

```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE})
@Documented
@Import({DelegatingWebMvcConfiguration.class})
public @interface EnableWebMvc {
}
```

```java
@Configuration
public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {
    ...
}
```

#### 接口编程方式

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Import({CachingConfigurationSelector.class})
public @interface EnableCaching {
    ...
}
```

```java
public class CachingConfigurationSelector extends AdviceModeImportSelector<EnableCaching> {
    public String[] selectImports(AdviceMode adviceMode) {
        switch(adviceMode) {
        case PROXY:
            return this.getProxyImports();
        case ASPECTJ:
            return this.getAspectJImports();
        default:
            return null;
        }
    }
}
```

### 自定义@Enable 模块

#### 基于注解驱动实现 - `@EnableHelloWorld`

#### 基于接口驱动实现 - `@EnableService`

`HelloWorldImportSelector` -&gt; `HelloWorldConfiguration` -&gt; `helloWorld`

## Spring 条件装配

### 举例

* `@Profile`
* `@Conditional`

### 实现方式

#### 配置方式 - `@Profile`

#### 编程方式 - `@Conditional`

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Conditional({OnClassCondition.class})
public @interface ConditionalOnClass {
    Class<?>[] value() default {};

    String[] name() default {};
}
```

### 自定义条件装配

#### 基于配置方法实现 - @Profile

#### 基于编程方式- `@ConditionalOnSystemProperty`

```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE, ElementType.METHOD})
@Documented
@Conditional(OnSystemPropertyCondition.class)
public @interface ConditionalOnSystemProperty {

    /**
     * Java 系统属性名称
     * @return
     */
    String name();

    /**
     * Java 系统属性值
     * @return
     */
    String value();
}
```

```java
public class OnSystemPropertyCondition implements Condition {
    @Override
    public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {

        Map<String, Object> attributes = metadata.getAnnotationAttributes(ConditionalOnSystemProperty.class.getName());

        String propertyName = String.valueOf(attributes.get("name"));
        String propertyValue = String.valueOf(attributes.get("value"));

        String javaPropertyValue = System.getProperty(propertyName);

        return javaPropertyValue.equals(propertyValue);
    }
}

```



