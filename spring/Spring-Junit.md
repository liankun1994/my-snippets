# Spring整合Junit

## jar包

```
junit-4.9.jar
spring-test-4.1.3.RELEASE.jar
```

## 示例

### applicationContext.xml

```xml
<bean id="obj" class="com.liankun.MyObject" />
```

### MyObject.java

```java
package com.liankun;
public class MyObject {
    
}
```

### MyJunit.java

```java
package com.liankun;

import javax.annotation.Resource;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext.xml")
public class MyJunit {
	@Resource(name = "obj")
	private MyObject myObject;

	@Test
	public void myTest() {
		// ...
	}
}
```

