# Date相关类的使用

标签（空格分隔）： Date 日期 常用类

---
[TOC]
### 几个处理时间有关的类
- **java.util.Date**;
- **java.sql.Date**;
- **java.text.DateFormat**;
- **java.util.Calendar**;

---

### java.util.Date与java.sql.Date的区别与联系
#### **联系**
java.util.Date是java.sql.Date的**父类**
#### **区别**
java.util.Date能表示**年月日时分秒**
java.sql.Date只能表示**年月日**
#### **互相转化**
##### **String-->java.sql.Date**
```java
//java.sql.Date必须调用有参构造器初始化
java.sql.Date d1 = new java.sql.Date(14791105538317L); 
java.sql.Date d2 = d1.valueOf("2016-11-14");
```
##### **String-->java.util.Date**
```java
public static long parse(String s)//不推荐了
```
### DateFormat
#### 创建对象
因为Dateformat是**抽象类**，不能直接创建对象，需要通过其子类创建对象
```java
DateFormat df = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
```
#### **String-->Date**
```java
try{
    java.util.Date d = df.parse("2016-11-14 19:30:13")//此处的格式与上边创建对象时的格式要对应
}catch(ParseException e){//格式不正确，会抛出异常
    e.printStackTrace();
}
```
#### **Date-->String**
- 最简单的就是直接利用toString()方法输出（略）
- 利用DateFormat转成自己想要的格式输出
```java
DateFormat df2 = new SimpleDateFormat("yyyy/MM/dd hh/mm/ss");
System.out.println(df2.format(d1);
```
### **Calendar类**
#### 创建对象
有**两种**方法创建对象
```java
Calendar cal1 =Calendar.getInstance();//利用getInstance()方法创建
Calendar cal2 = new GregorianCalendar();//利用子类创建
```
#### 获取属性
```java
System.out.println(cal.get(Calendar.YEAR));//获取年份
System.out.println(cal.get(Calendar.MONTH));//获取月份
System.out.println(cal.get(Calendar.DAY_OF_MONTH));//获取日
System.out.println(cal.get(Calendar.DATE));获取日
System.out.println(cal.getActualMaximum(Calendar.DATE));//获取指定日期一月中的第一天
System.out.println(cal.getActualMinimum(Calendar.DATE));//获取指定日期一月中的最后一天
```
#### 修改属性
```java
//将日期设定为2012-7-21
cal.set(Calendar.YEAR,2012);
cal.set(Calendar.MONTH,7);
cal.set(Calendar.DATE,21);
```
#### **String-->Calendar**
##### **String-->Date**
```java
java.sql.Date d = java.sql.Date.valueOf("2015-12-1");
```
##### **Date-->Calendar**
```java
cal.setTime(d);
```



