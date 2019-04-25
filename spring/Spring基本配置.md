# Spring 基本配置

## 文件头

### applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
	xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
	http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd
	http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
	http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.0.xsd">
```

## Bean作用范围

### applicationContext.xml

```xml
<!-- 单例模式 -->
<bean id="daoTest" class="com.liankun.dao.DaoTest" scope="singleton"></bean>
```

```xml
<!-- 多例模式 -->
<bean id="daoTest" class="com.liankun.dao.DaoTest" scope="prototype"></bean>
```

```xml
<!-- 存入request域 -->
<bean id="daoTest" class="com.liankun.dao.DaoTest" scope="request"></bean>
```

```xml
<!-- 存入session域 -->
<bean id="daoTest" class="com.liankun.dao.DaoTest" scope="session"></bean>
```

```xml
<!-- 在porlet环境使用，供子系统调用。 -->
<bean id="daoTest" class="com.liankun.dao.DaoTest" scope="globalsession"></bean>
```

## Bean生命周期

### applicationContext.xml

```xml
<bean id="daoTest" class="com.liankun.dao.DaoTest" init-method="setup" destroy-method="destory" />
```

### DaoTest.java

```java
package com.liankun.dao;

public class DaoTest {
	public void setup() {
		System.out.println("DaoTest init.");
	}
	public void destory() {
		System.out.println("DaoTest destory.");
	}
}
```

### ServiceTest.java

```java
ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
DaoTest daoTest = (DaoTest) applicationContext.getBean("daoTest");
//...
applicationContext.close();
```

## 从项目路径读取配置文件

### ServiceTest.java

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
```

## 从文件系统读取配置文件

### ServiceTest.java

```java
ApplicationContext applicationContext = new FileSystemXmlApplicationContext("C:\\applicationContext.xml");
```

## 一次加载多个配置文件

### MyTest.java

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml","applicationContext2.xml");
```

## 引入其他配置文件

### applicationContext.xml

```xml
<import resource="applicationContext2.xml"/>
```

