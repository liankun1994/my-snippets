# Spring AOP

## Jar包

```
aopalliance-1.0.jar
aspectjweaver-1.6.11.jar
spring-aop-4.1.3.RELEASE.jar
spring-aspects-4.1.3.RELEASE.jar
```

### applicationContext.xml

```xml
<bean id="myObject" class="com.liankun.MyObject" />
<bean id="myAspect" class="com.liankun.MyAspect" />

<aop:config>
    <aop:pointcut expression="execution(* com.liankun.MyObject.doSomething(..))" id="myPointcut"/>
    <aop:aspect ref="myAspect">
        <aop:before method="checkPri" pointcut-ref="myPointcut"/>
    </aop:aspect>
</aop:config>
```

### MyObject.java

```java
package com.liankun;

public class MyObject {
	public void doSomething() {
		System.out.println("程序执行");
	}
}
```

### MyAspect.java

```java
package com.liankun;

public class MyAspect {
	public void checkPri() {
		System.out.println("校验权限");
	}
}
```

### MyTest.java

```java
myObject.doSomething();
```

