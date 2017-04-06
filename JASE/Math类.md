# Math类

标签（空格分隔）： JavaSE

---
[TOC]
### 静态导入
```java
import static java.util.Math.*;
```
这样就可以直接如下调用Math类下的方法
```java
System.out.println(random()*100);
```
但在当前类有同名方法时，会优先调用本类的同名方法（就近原则）
### 常用方法
#### 取绝对值
```java
System.out.println(Math.abs(-9));//输出：9
```
#### 开平方
````java
System.out.println(Math.sqr(9);//输出：3

````
#### 向下取值，向上取值
```java
//向下取值
System.out.println(Math.floor(90.9999));//输出：90.0
System.out.println(Math.floor(-90.0001));//输出：-91.0

//向上取值
System.out.println(Math.cei(90.0001));//输出：91.0
System.out.println(Math.cei(-90.9999));//输出：-90.0
```
#### 四舍五入
```java
System.out.println(Math.round(90.6));//输出：91
```
#### 随机数
```java
System.out.println(Math.random());
```
#### 次幂
```java
System.out.println(Math.pow(3,2));//输出：9
```




