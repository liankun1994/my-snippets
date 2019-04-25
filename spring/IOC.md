# Spring IOC

## 示例

### applicationContext.xml

```xml
<bean id="daoTest" class="com.liankun.dao.DaoTest"></bean>
```

### DaoTest.java

```java
package com.liankun.dao;

public class DaoTest {
	public void save() {
		System.out.println("DaoTest save.");
	}
}
```

### ServiceTest.java

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
DaoTest daoTest = (DaoTest) applicationContext.getBean("daoTest");
daoTest.save();
```

## 构造方法注入

### applicationContext.xml

```xml
<bean id="daoTest" class="com.liankun.dao.DaoTest" >
    <constructor-arg name="number" value="001"/>
    <constructor-arg name="name" value="张三" />
</bean>
```

### DaoTest.java

```java
package com.liankun.dao;

public class DaoTest {
	private String number;
	private String name;
	public DaoTest(String number, String name) {
		this.number = number;
		this.name = name;
	}
}
```

## 构造方法注入对象

### applicationContext.xml

```xml
<bean id="daoRef" class="com.liankun.dao.DaoTestRef">
    <constructor-arg name="pro" value="myValue"/>
</bean>
<bean id="dao" class="com.liankun.dao.DaoTest">
    <constructor-arg name="obj" ref="daoRef" />
</bean>
```

### DaoTestRef.java

```java
package com.liankun.dao;

public class DaoTest {
	private DaoTestRef obj;
	public DaoTest(DaoTestRef obj) {
		this.obj = obj;
	}
}
```

### DaoTest.java

```java
package com.liankun.dao;

public class DaoTestRef {
	private String pro;
	public DaoTestRef(String pro) {
		this.pro = pro;
	}
}
```

### ServiceTest.java

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
DaoTest daoTest = (DaoTest) applicationContext.getBean("dao");
```

## set方法注入

### applicationContext.xml

```xml
<bean id="daoTest" class="com.liankun.dao.DaoTest">
	<property name="name" value="张三"></property>
</bean>
```

### DaoTest.java

```java
package com.liankun.dao;

public class DaoTest {
	private String name;
	public void setName(String name) {
		this.name = name;
	}
}
```

## set方法注入对象

### applicationContext.xml

```xml
<bean id="daoRef" class="com.liankun.dao.DaoTestRef">
    <property name="pro" value="myValue" />
</bean>
<bean id="dao" class="com.liankun.dao.DaoTest">
    <property name="obj" ref="daoRef" />
</bean>
```

### DaoTestRef.java

```java
package com.liankun.dao;

public class DaoTestRef {
	private String pro;
	public void setPro(String pro) {
		this.pro = pro;
	}
}
```

### DaoTest.java

```java
package com.liankun.dao;

public class DaoTest {
	private DaoTestRef obj;
	public void setObj(DaoTestRef obj) {
		this.obj = obj;
	}
}
```

### ServiceTest.java

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
DaoTest daoTest = (DaoTest) applicationContext.getBean("dao");
```

## P名称空间注入

### applicationContext.xml

```xml
<beans xmlns:p="http://www.springframework.org/schema/p" >
	<bean id="dao" class="com.liankun.dao.DaoTest" p:pro="myValue" />
</beans>
```

### DaoTest.java

```java
package com.liankun.dao;

public class DaoTest {
	private String pro;
	public void setPro(String pro) {
		this.pro = pro;
	}
}
```

## P名称空间注入对象

### applicationContext.xml

```xml
<beans xmlns:p="http://www.springframework.org/schema/p" >
	<bean id="daoRef" class="com.liankun.dao.DaoTestRef" p:pro="myValue" />
	<bean id="dao" class="com.liankun.dao.DaoTest" p:obj-ref="daoRef" />
</beans>
```

### DaoTestRef.java

```java
package com.liankun.dao;

public class DaoTestRef {
	private String pro;
	public void setPro(String pro) {
		this.pro = pro;
	}
}
```

### DaoTest.java

```java
package com.liankun.dao;

public class DaoTest {
	private DaoTestRef obj;
	public void setObj(DaoTestRef obj) {
		this.obj = obj;
	}
}
```

### ServiceTest.java

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
DaoTest daoTest = (DaoTest) applicationContext.getBean("dao");
```

## SpEL注入

### applicationContext.xml

```xml
<bean id="daoRef" class="com.liankun.dao.DaoTestRef">
    <property name="pro" value="#{'Properties'}" />
</bean>
<bean id="dao" class="com.liankun.dao.DaoTest">
    <property name="name" value="#{'id001'}" />
    <property name="price" value="#{3000.2}" />
    <property name="obj" value="#{daoRef}" /> 	
</bean>
```

### DaoTestRef.java

```java
package com.liankun.dao;

public class DaoTestRef {
	private String pro;
	public void setPro(String pro) {
		this.pro = pro;
	}
}
```

### DaoTest.java

```java
package com.liankun.dao;

public class DaoTest {
	private String name;
	private double price;
	private DaoTestRef obj;
	public void setName(String name) {
		this.name = name;
	}
	public void setPrice(double price) {
		this.price = price;
	}
	public void setObj(DaoTestRef obj) {
		this.obj = obj;
	}
}
```

### ServiceTest.java

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
DaoTest daoTest = (DaoTest) applicationContext.getBean("dao");
```

## SpEL从其他对象的成员注入

### applicationContext.xml

```xml
<bean id="util" class="com.liankun.util.Util" />
<bean id="dao" class="com.liankun.dao.DaoTest">
    <property name="name" value="#{util.pro}" />
    <property name="price" value="#{util.method()}" />
</bean>
```

### Util.java

```java
package com.liankun.util;

public class Util {
	private String pro;
	public String getPro() {
		return "Properties";
	}
	public double method() {
		return 100.20D;
	}
}
```

### DaoTest.java

```java
package com.liankun.dao;

public class DaoTest {
	private String name;
	private double price;
	public void setName(String name) {
		this.name = name;
	}
	public void setPrice(double price) {
		this.price = price;
	}
}
```

### ServiceTest.java

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
DaoTest daoTest = (DaoTest) applicationContext.getBean("dao");
```

## 注入数组

### applicationContext.xml

```xml
<bean id="collectionBean" class="com.liankun.CollectionBean">
    <property name="arrs">
        <list>
            <value>a</value>
            <value>b</value>
            <value>c</value>
        </list>
    </property>
</bean>
```

### CollectionBean.java

```java
package com.liankun;

public class CollectionBean {
	private String[] arrs;
	public void setArrs(String[] arrs) {
		this.arrs = arrs;
	}
}
```

### MyTest.java

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
CollectionBean collectionBean = (CollectionBean) applicationContext.getBean("collectionBean");
```

## 注入对象数组

### applicationContext.xml

```xml
<bean id="a" class="com.liankun.MyObject"></bean>
<bean id="b" class="com.liankun.MyObject"></bean>
<bean id="c" class="com.liankun.MyObject"></bean>

<bean id="collectionBean" class="com.liankun.CollectionBean">
    <property name="arrs">
        <list>
            <ref bean="a" />
            <ref bean="b" />
            <ref bean="c" />
        </list>
    </property>
</bean>
```

### MyObject.java

```java
package com.liankun;

public class MyObject {
    
}
```

### CollectionBean.java

```java
package com.liankun;

public class CollectionBean {
	private MyObject[] arrs;
	public void setArrs(MyObject[] arrs) {
		this.arrs = arrs;
	}
}
```

### MyTest.java

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
CollectionBean collectionBean = (CollectionBean) applicationContext.getBean("collectionBean");
```

## 注入List

### applicationContext.xml

```xml
<bean id="collectionBean" class="com.liankun.CollectionBean">
    <property name="list">
        <list>
            <value>a</value>
            <value>b</value>
            <value>c</value>
        </list>
    </property>
</bean>
```

### CollectionBean.java

```java
package com.liankun;
import java.util.List;

public class CollectionBean {
	private List<String> list;
	public void setList(List<String> list) {
		this.list = list;
	}
}
```

### MyTest.java

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
CollectionBean collectionBean = (CollectionBean) applicationContext.getBean("collectionBean");
```

## 注入对象List

### applicationContext.xml

```xml
<bean id="a" class="com.liankun.MyObject"></bean>
<bean id="b" class="com.liankun.MyObject"></bean>
<bean id="c" class="com.liankun.MyObject"></bean>

<bean id="collectionBean" class="com.liankun.CollectionBean">
    <property name="list">
        <list>
            <ref bean="a" />
            <ref bean="b" />
            <ref bean="c" />
        </list>
    </property>
</bean>
```

### MyObject.java

```java
package com.liankun;

public class MyObject {
    
}
```

### CollectionBean.java

```java
package com.liankun;
import java.util.List;

public class CollectionBean {
	private List<MyObject> list;
	public void setList(List<MyObject> list) {
		this.list = list;
	}
}
```

### MyTest.java

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
CollectionBean collectionBean = (CollectionBean) applicationContext.getBean("collectionBean");
```

## 注入Set

### applicationContext.xml

```xml
<bean id="collectionBean" class="com.liankun.CollectionBean">
    <property name="set">
        <set>
            <value>a</value>
            <value>b</value>
            <value>c</value>
        </set>
    </property>
</bean>
```

### CollectionBean.java

```java
package com.liankun;
import java.util.Set;

public class CollectionBean {
	private Set<String> set;
	public void setSet(Set<String> set) {
		this.set = set;
	}
}
```

### MyTest.java

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
CollectionBean collectionBean = (CollectionBean) applicationContext.getBean("collectionBean");
```

## 注入对象Set

### applicationContext.xml

```xml
<bean id="a" class="com.liankun.MyObject"></bean>
<bean id="b" class="com.liankun.MyObject"></bean>
<bean id="c" class="com.liankun.MyObject"></bean>

<bean id="collectionBean" class="com.liankun.CollectionBean">
    <property name="set">
        <set>
            <ref bean="a" />
            <ref bean="b" />
            <ref bean="c" />
        </set>
    </property>
</bean>
```

### MyObject.java

```java
package com.liankun;

public class MyObject {
    
}
```

### CollectionBean.java

```java
package com.liankun;
import java.util.Set;

public class CollectionBean {
	private Set<MyObject> set;
	public void setSet(Set<MyObject> set) {
		this.set = set;
	}
}
```

### MyTest.java

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
CollectionBean collectionBean = (CollectionBean) applicationContext.getBean("collectionBean");
```

## 注入Map

### applicationContext.xml

```xml
<bean id="collectionBean" class="com.liankun.CollectionBean">
    <property name="map">
        <map>
            <entry key="a" value="1" />
            <entry key="b" value="2" />
            <entry key="c" value="3" />
        </map>
    </property>
</bean>
```

### CollectionBean.java

```java
package com.liankun;
import java.util.Map;

public class CollectionBean {
	private Map<String,String> map;
	public void setMap(Map<String, String> map) {
		this.map = map;
	}
}
```

### MyTest.java

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
CollectionBean collectionBean = (CollectionBean) applicationContext.getBean("collectionBean");
```

## 注入对象Map

### applicationContext.xml

```xml
<bean id="a" class="com.liankun.MyObject"></bean>
<bean id="b" class="com.liankun.MyObject"></bean>
<bean id="c" class="com.liankun.MyObject"></bean>
<bean id="d" class="com.liankun.MyObject"></bean>

<bean id="collectionBean" class="com.liankun.CollectionBean">
    <property name="map">
        <map>
            <entry key-ref="a" value-ref="c" />
            <entry key-ref="b" value-ref="d" />
        </map>
    </property>
</bean>
```

### MyObject.java

```java
package com.liankun;

public class MyObject {
    
}
```

### CollectionBean.java

```java
package com.liankun;
import java.util.Map;

public class CollectionBean {
	private Map<MyObject,MyObject> map;
	public void setMap(Map<MyObject, MyObject> map) {
		this.map = map;
	}
}
```

### MyTest.java

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
CollectionBean collectionBean = (CollectionBean) applicationContext.getBean("collectionBean");
```