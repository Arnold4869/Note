[TOC]
# 1. spring搭建过程
## 1.1 简略步骤
1. 添加需要的jar包到项目并`build path`
2. 在`src`根目录下添加一个`applicationContext.xml`的配置文件，具体格式参见
3. **解析**配置文件

## 1.2 配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>
        <!-- collaborators and configuration for this bean go here -->
    </bean>
</beans>
```
## 1.3 如何解析配置文件

```java
//解析配置文件
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
//获取对象
UserDao userDao = (UserDao) applicationContext.getBean("UserDao");
```
**在解析配置文件时已经默认创建了对象**

# 2. xml配置文件详解
## 2.1 bean

1. id
2. class
3. factory-bean
4. factory-method
5. prototype
6. abstract
7. parent
8. init-method
9. destroy-method

### 2.1.1 bean成员变量的赋值方式
#### value
`<value../>`用于指定基本类型及其包装、字符串类型的参数值
#### ref
`<ref.../>`用于指定成员变量是另一个`bean`实例的情况

```xml
<bean id="chinese" class="org.crazyit.app.service.impl.Chinese">
	<!-- 指定容器中id为stealAxe的bean作为调用setAxe()方法的参数 -->
	<property name="axe" ref="steelAxe"/>
</bean>
```
#### bean
使用自动装配来注入合作者`bean`
#### list、set、map和props
需要调用的形参类型为集合时，可以使用`<list.../>`、`<set.../>`、`<map.../>`和`<props.../>`分别来设置类型为`List`、`Set`、`Map`、`Properties`的集合参数值
## import
# 3. Bean生命周期
`Spring`只会管理`singleton`作用域的的`Bean`的生命周期，对于`prototype`作用域的`Bean`，`Spring`仅仅负责创建，当容器创建了`Bean`实例之后，`Bean`实例完全交给客户端代码管理。

## init
可以在`Bean`的属性`init-method`来指定初始化时加载的方法
## destroy
`destroy-method`来指定销毁时执行的方法；也可以使用`DisposableBean`接口
# 4. AOP
## 术语
连接点：是程序执行过程中**可以**应用通知的**所有点**

切点：用来筛选连接点，定义了**被应用**的所在位置，其实就是**实际被应用**的地方。**切点来筛选连接点，选中想要的方法**

通知：切面必须完成的工作被称之为通知,有五种(`Before`,`After`,`After-return`,`After-throwing`,`Around`)

切面：它是通知和切点的结合。通知和切点共同定义了关于切面的全部内容（它是什么，在何时，和何处完成其功能）。**切面就是切点和通知统称起来的东西。具体到了某个方法的某个位置（执行前或者执行后），要做什么事（具体的事情由通知定义）**

## 编写切点
**切点需要在配置文件中声明**

```
execution(* com.bjsxt.arnold.Student.play(..)
```
# 5. annotation

常用注解
- `@Component`:标注一个普通的 Spring Bean 类
- `@Controller`:标注一个控制器组件类
- `@Service`:标注一个业务逻辑组件类
- `@Repository`:标注一个Dao组件类

# 6. 事务管理

## 6.1 事务管理器的选择
接口是 `PlatformTransactionManager`，具体的实现类根据不同情况选择
![事务管理器的选择](https://raw.githubusercontent.com/Arnold4869/note/master/images/TransactionManagerforSpring.png)

## 6.2 事务的隔离级别
原因：**不同级别的事务会有不同的隐患**

脏读：一个事务读取了另一个事务改写但还未提交的数据，如果这些数据被回滚，则读到的数据是无效的。

不可重复读：在同一事务中，多次读取同一数据返回的结果有所不同。

幻读：一个事务读取了几行记录后，另一个事务插入一些记录，幻读就发生了。再后来查询中，第一个事务就会发现有些原来没有的记录。

### 几种隔离级别
- Default：Spring 采用数据库默认隔离级别
- READ_UNCOMMITED:允许你读取还未提交的改变了的数据。可能导致脏、幻、不可重复读
- READ_COMMITTED：允许在并发事务已经提交后读取。可防止脏读，但幻读、不可重复读仍会发生
- REPEATABLE_READ：对相同字段的多次读取是一致的，除非数据被事务本身改变。可防止脏读、不可重复读，但幻读仍可能发生
- SERIALIZABLE：完全服从 ACID 原则，确保部发生脏、乱、不可重复读。但速度是最慢的。

## 6.2 事务的传播行为
**用来解决业务层方法之间的相互调用问题**。因为事务是控制在业务层的，如果业务层之间多个方法调用，事务的级别就乱了。

- **PROPAGATION_REQUIRED** 如果存在一个事务，则支持当前事务。如果没有事务则开启一个新的事务。

- **PROPAGATION_SUPPORTS** 如果存在一个事务，支持当前事务。如果没有事务，则非事务的执行。但是对于事务同步的事务管理器，PROPAGATION_SUPPORTS与不使用事务有少许不同。

- **PROPAGATION_MANDATORY** 如果已经存在一个事务，支持当前事务。如果没有一个活动的事务，则抛出异常。

- **PROPAGATION_REQUIRES_NEW** 总是开启一个新的事务。如果一个事务已经存在，则将这个存在的事务挂起。

- **PROPAGATION_NOT_SUPPORTED** 总是非事务地执行，并挂起任何存在的事务。

- **PROPAGATION_NEVER** 总是非事务地执行，如果存在一个活动事务，则抛出异常

- **PROPAGATION_NESTED** 如果一个活动的事务存在，则运行在一个嵌套的事务中. 如果没有活动事务, 则按TransactionDefinition.PROPAGATION_REQUIRED 属性执行

## 6.3 Spring 控制事务的方式
- 编程式事务管理
		手动编写代码进行事务管理.(很少使用)
- 声明式事务管理:
		1. 基于TransactionProxyFactoryBean的方式.(很少使用)
      **需要为每个进行事务管理的类,配置一个TransactionProxyFactoryBean进行增强.**
		2. 基于AspectJ的XML方式.(经常使用)
			**一旦配置好之后,类上不需要添加任何东西**
		3. 基于注解方式.(经常使用)
			**配置简单,需要在业务层上添加一个@Transactional的注解.**
