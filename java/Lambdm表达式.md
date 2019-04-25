# Lambdm表达式

## 无参数无返回值

### MyInterface.java

```java
package com.liankun;

public interface MyInterface {
	public void method();
}
```

### Main.java

```java
MyInterface myInterface = () -> System.out.println("hello");
myInterface.method();
```

## 有一个参数

### MyInterface.java

```java
package com.liankun;

public interface MyInterface {
	public int method(int a);
}
```

### Main.java

```java
// 一个参数不需要括号
MyInterface myInterface = a -> a * 2;
myInterface.method(3);
```

## 有参数有返回值

### MyInterface.java

```java
package com.liankun;

public interface MyInterface {
	public int method(int a,int b);
}
```

### Main.java

```java
MyInterface myInterface = (a, b) -> a + b;
myInterface.method(1, 5);
```

```java
// 参数可以带类型名
MyInterface myInterface = (int a, int b) -> a + b;
myInterface.method(1, 5);
```

```java
// 表达式多句写法
MyInterface myInterface = (a, b) -> {
    return a + b;
};
myInterface.method(1, 5);
```

