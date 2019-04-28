# Spring AOP

## Jar包

```
aopalliance-1.0.jar
aspectjweaver-1.6.11.jar
spring-aop-4.1.3.RELEASE.jar
spring-aspects-4.1.3.RELEASE.jar
```

## 前置通知

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

## 输出切入点

### MyAspect.java

```java
package com.liankun;
import org.aspectj.lang.JoinPoint;

public class MyAspect {
	public void checkPri(JoinPoint joinPoint) {
		System.out.println(joinPoint);
	}
}
```

## 后置通知

### applicationContext.xml

```xml
<bean id="myObject" class="com.liankun.MyObject" />
<bean id="myAspect" class="com.liankun.MyAspect" />

<aop:config>
    <aop:pointcut expression="execution(* com.liankun.MyObject.doSomething(..))" id="myPointcut"/>
    <aop:aspect ref="myAspect">
        <aop:after-returning method="writeLog" pointcut-ref="myPointcut"/>
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
	public void writeLog() {
		System.out.println("日志记录");
	}
}
```

### MyTest.java

```java
myObject.doSomething();
```

## 获取返回值

### applicationContext.xml

```xml
<bean id="myObject" class="com.liankun.MyObject" />
<bean id="myAspect" class="com.liankun.MyAspect" />

<aop:config>
    <aop:pointcut expression="execution(* com.liankun.MyObject.doSomething(..))" id="myPointcut"/>
    <aop:aspect ref="myAspect">
        <aop:after-returning method="writeLog" pointcut-ref="myPointcut" returning="myReturn"/>
    </aop:aspect>
</aop:config>
```

### MyObject.java

```java
package com.liankun;

public class MyObject {
	public String doSomething() {
		System.out.println("程序执行");
		return "returnValue";
	}
}
```

### MyAspect.java

```java
package com.liankun;

import org.aspectj.lang.JoinPoint;

public class MyAspect {
	public void writeLog(Object myReturn) {
		System.out.println("日志记录");
		System.out.println(myReturn);
	}
}
```

### MyTest.java

```java
myObject.doSomething();
```

## 环绕通知

### applicationContext.xml

```xml
<bean id="myObject" class="com.liankun.MyObject" />
<bean id="myAspect" class="com.liankun.MyAspect" />

<aop:config>
    <aop:pointcut expression="execution(* com.liankun.MyObject.doSomething(..))" id="myPointcut"/>
    <aop:aspect ref="myAspect">
        <aop:around method="around" pointcut-ref="myPointcut"/>
    </aop:aspect>
</aop:config>
```

### MyObject.java

```java
package com.liankun;

public class MyObject {
	public String doSomething() {
		System.out.println("程序执行");
		return "returnValue";
	}
}
```

### MyAspect.java

```java
package com.liankun;

public class MyAspect {
	public Object around(ProceedingJoinPoint joinPoint) throws Throwable {
		System.out.println("程序开始");
		Object obj = joinPoint.proceed();
		System.out.println("程序结束");
		return obj;
	}
}
```

### MyTest.java

```java
myObject.doSomething();
```

## 异常抛出通知

### applicationContext.xml

```xml
<bean id="myObject" class="com.liankun.MyObject" />
<bean id="myAspect" class="com.liankun.MyAspect" />

<aop:config>
    <aop:pointcut expression="execution(* com.liankun.MyObject.doSomething(..))" id="myPointcut"/>
    <aop:aspect ref="myAspect">
        <aop:after-throwing method="afterThrowing" pointcut-ref="myPointcut" />
    </aop:aspect>
</aop:config>
```

### MyObject.java

```java
package com.liankun;

public class MyObject {
	public void doSomething() {
		int a = 2/0;
	}
}
```

### MyAspect.java

```java
package com.liankun;

public class MyAspect {
	public void afterThrowing() {
		System.out.println("发现异常");
	}
}
```

### MyTest.java

```java
myObject.doSomething();
```

## 获取异常信息

### applicationContext.xml

```xml
<bean id="myObject" class="com.liankun.MyObject" />
<bean id="myAspect" class="com.liankun.MyAspect" />

<aop:config>
    <aop:pointcut expression="execution(* com.liankun.MyObject.doSomething(..))" id="myPointcut"/>
    <aop:aspect ref="myAspect">
        <aop:after-throwing method="afterThrowing" pointcut-ref="myPointcut" throwing="myException"/>
    </aop:aspect>
</aop:config>
```

### MyObject.java

```java
package com.liankun;

public class MyObject {
	public void doSomething() {
		int a = 2/0;
	}
}
```

### MyAspect.java

```java
package com.liankun;

public class MyAspect {
	public void afterThrowing(Throwable myException) {
		System.out.println("发现异常");
		System.out.println(myException.getMessage());
	}
}
```

### MyTest.java

```java
myObject.doSomething();
```

## 最终通知

### applicationContext.xml

```xml
<bean id="myObject" class="com.liankun.MyObject" />
<bean id="myAspect" class="com.liankun.MyAspect" />

<aop:config>
    <aop:pointcut expression="execution(* com.liankun.MyObject.doSomething(..))" id="myPointcut"/>
    <aop:aspect ref="myAspect">
        <aop:after method="after" pointcut-ref="myPointcut" />
    </aop:aspect>
</aop:config>
```

### MyObject.java

```java
package com.liankun;

public class MyObject {
	public void doSomething() {
		int a = 2/0;
	}
}
```

```java
package com.liankun;

public class MyObject {
	public void doSomething() {
		System.out.println("程序运行");
	}
}
```

### MyAspect.java

```java
package com.liankun;

public class MyAspect {
	public void after() {
		System.out.println("程序结束");
	}
}
```

### MyTest.java

```java
myObject.doSomething();
```

## 切入点表达式

```xml
<!-- ([访问修饰符] 方法返回值 包名.类名.方法名(参数)) -->
<aop:pointcut expression="execution(public void com.liankun.myObject.doSomething(..))" id="myPointcut" />
```

```xml
<!-- 访问修饰符可以省略 -->
<aop:pointcut expression="execution(* com.liankun.myObject.doSomething(..))" id="myPointcut" />
```

```xml
<!-- 返回类型、包名、类名和方法名都可以用*代表任意。 -->
<aop:pointcut expression="execution(* *.liankun.myObject.doSomething(..))" id="myPointcut" />
```

```xml
<aop:pointcut expression="execution(* com.*.myObject.doSomething(..))" id="myPointcut" />
```

```xml
<aop:pointcut expression="execution(* com.liankun.*.doSomething(..))" id="myPointcut" />
```

```xml
<aop:pointcut expression="execution(* com.liankun.myObject.*(..))" id="myPointcut" />
```

```xml
<aop:pointcut expression="execution(* *.*.*.*(..))" id="myPointcut" />
```

```xml
<!-- myObject和它的子类都增强。 -->
<aop:pointcut expression="execution(* com.liankun.myObject+.doSomething(..))" id="myPointcut" />
```

```xml
<!-- 表示liankun下的所有子包的myObject.doSomething方法 -->
<aop:pointcut expression="execution(* com.liankun..myObject.doSomething(..))" id="myPointcut" />
```

```xml
<!-- 增强以do名字开头的方法。 -->
<aop:pointcut expression="execution(* com.liankun.myObject.do*(..))" id="myPointcut" />
```