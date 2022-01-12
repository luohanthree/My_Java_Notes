# 接口、lambda表达式、内部类以及代理 



# 接口

### 引言

接口用来表示***应该做什么*** 而不是***怎么做*** 

### 概念

什么是接口：接口不是类，而是对希望符合这一接口的类的**需求**

### 属性

接口没有**实例**<!--及没有实例化字段，不会实现方法--> 可以将接口与抽象类进行类比；

实现接口关键词：**implements**

接口不是类，不能使用new实例化

可以声明接口变量，必须引用实现这个接口的类对象

```java
Comparable x = new Employee(...){}
```

可以使用instanceof来检查一个类是否实现了接口

```java
if (anObject instanceof Comparable)
```

接口中可以包含常量，默认为：

```java
public static final
```

### 接口与抽象类 

使用抽象类表示通用属性存在一个问题，一个类只能扩展**一个类**

但每个类可以实现多个接口 。

### 接口中的方法

#### 静态方法

java8中允许接口中实现静态方法。

通常做法是将静态方法放在伴随类中。

#### 默认方法

可以为接口提供 **默认** 实现。用**default**修饰符标记

```java
public interface Comparable<T> {
    default int compareTo( return 0;)
}
```

被default修饰的方法可以不必在实现这一接口的类中实现。



#### 解决默认方法冲突

1. **超类优先**：如果超类中提供了一个具体方法，具有相同的签名的默认方法会被忽略；

2. **接口冲突**：如果一个接口提供了一个默认方法，另一个接口提供了一个同名而且参数类型(不论是否是默认参数)相同的方法，必须覆盖这个解决冲突。

   实例：

   ```java
   interface Person {
       default String getName() { return ""; };
   }
   
   interface Named {
       default String getName() { return getClass() + "_" + hashCode();}
   }
   
   class Student implements Person,Named {...}
   ```

   类会继承Person ,Named接口提供的两个不一致的getName方法。Java编译器会报告错误，让程序员来解决二义性问题。只需在Student类中提供一个getName方法即可。在这个方法中，可以选择这两个方法中的一个

   ```java
   class Student implements Person,Named {
       public String getName() { return Person.super.getName(); }
       ...
   }
   ```

3. **类优先**：如果一个类扩展了一个超类，同时实现了一个接口 ，并从接口超类和接口继承了同样的方法。

   如，假设Person是一个类：

   ```java
   class Student extends Person implements Named {...}
   ```

   这种情况下只会考虑，接口的所有默认方法都会被忽略。

   