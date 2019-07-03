# 数据相关

## JDBC

### 依赖

```markup
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
```

### 数据源

* `javax.sql.DataSource`

### JdbcTemplate

### 自动装配

* `DataSourceAutoConfiguration`

## JPA

### 依赖

```markup
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

### 实体映射关系

* `@javax.persistence.OneToOne`
* `@javax.persistence.OneToMany`
* `@javax.persistence.ManyToOne`
* `@javax.persistence.ManyToMany`
* ...

### 实体操作

* `javax.persistence.EntityManager`

### 自动装配

* `HibernateJpaAutoConfiguration`

## 事务

### 依赖

```markup
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-tx</artifactId>
</dependency>
```

### Spring事务抽象

* `PlatformTransactionManager`

### JDBC事务处理

* `DataSourceTransactionManager`

### 自动装配

* `TransactionAutoConfiguration`

