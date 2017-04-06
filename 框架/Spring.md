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
连接点：是程序执行过程中能够应用通知的**所有点**
切点：用来筛选连接点，定义了**被应用**的所在位置
通知：切面必须完成的工作被称之为通知,有五种(`Before`,`After`,`After-return`,`After-throwing`,`Around`)
切面：它是通知和切点的结合。通知和切点共同定义了关于切面的全部内容（它是什么，在何时，和何处完成其功能）
## 编写切点
`execution(* com.bjsxt.arnold.Student.play(..) and bean(teacher))`
# 5. annotation

常用注解
- `@Component`:标注一个普通的 Spring Bean 类
- `@Controller`:标注一个控制器组件类
- `@Service`:标注一个业务逻辑组件类
- `@Repository`:标注一个Dao组件类
