# Scala基础
[TOC]
## 使用Scala解释器

## 用var和val定义变量
### 使用val
```Scala
val answer = 8*5 +2
answer : Int = 42
```
以val定义的值实际上是一个常量——无法改变它的内容
```scala
answer = 0 //会报错
```
### 使用val
可以改变val定义的变量
```scala
var counter = 0
counter = 1
```
### 鼓励使用val
除非真的想要使用一个变量
### 不需要给出值或变量的类型
编译器会自动推导，但声明值或者变量的类型时，**不做初始化**会报错
但也可以在必要时指定
## 常用类型
### 基本类型全是类
Byte、Char、Short、Int、Long、Float、Double、Boolean
编译器自动对基本类型进行包装
## 使用操作符和函数
## 浏览Scaladoc