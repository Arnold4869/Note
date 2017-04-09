[TOC]

# 0. 主要架构
![Hibernate 架构](https://github.com/Arnold4869/note/blob/master/images/hibernate_architecture.jpg?raw=true "Hibrenate 架构")

# 1. 配置流程
1. 将hibernate包下/lib/requird下的包导入项目，如果非web项目还需要build path
2. 拷贝hibernate的主配置文件**hibernate.cfg.xml**到src根目录
3. 配置配置文件
4. 每个POJO类都应该在它所在的包内有个对应名字**Xxx.hbm.xml**的配置文件


# 2. 配置文件配置
## 2.1 主配置文件hibernate.cfg.xml

```xml
<hibernate-configuration>
    <session-factory>
        <!-- 最基本的数据库配置信息 -->
        <!-- 配置所选用的数据库 -->
        <property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>
        <!-- 配置数据库驱动位置 -->
        <property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
        <!-- URL -->
        <property name="hibernate.connection.url">jdbc:mysql://localhost:3306/hibernate</property>
        <!-- 用户名和密码 -->
        <property name="hibernate.connection.username">root</property>
        <property name="hibernate.connection.password">123456</property>
        <!-- 附加信息 -->
        <!-- 在控制台显示sql语句 -->
        <property name="hibernate.show_sql">true</property>
        <!--  -->
        <property name="hibernate.hbm2ddl.auto">create</property>

        <!-- 主配置文件挂载子配置文件 -->
        <!-- 将对应POJO类的配置文件*.hbm.xml配置到此处 -->
        <!-- resource注意是配置的文件路径，不是包名 -->
        <mapping resource="com/bjsxt/ly/po/User.hbm.xml" />

    </session-factory>
</hibernate-configuration>
```
### 2.1.1 数据库执行方式
- **create-drop**:当hibernate启动的时候会创建一张新表，但是当hibernate关闭的时候会将表删除
- **create**:当hibernate启动的时候会创建一张新表
- **update**:如果没有表就创建新的，如果已经存在就使用以前创建好的表,如果新增字段会直接修改表结构,如果减少一个字段该字段就不会被插入值，但当前列还在
- **validate**:不会给我们执行创建表的操作,验证表结构和实体结构是否统一，不能随便更改表结构

## 2.2 POJO类的配置文件*.hbm.xml

```xml
<hibernate-mapping package="com.bjsxt.ly.po">
    <class name="User">
        <!-- 设置主键 -->
        <id name="id" column="c_id">
            <generator class="assigned"></generator>
        </id>
        <property name="uname" length="20"></property>
        <property name="realname"></property>
        <property name="num" ></property>
        <property name="idnumber"></property>
        <property name="createtime"></property>
    </class>
</hibernate-mapping>
```
### 2.2.1 主键生成策略

```xml
<generator class= "assigned"></generator>
```
这里有以下几种可选：

**当要求是整型（int、long、short）之类时**

1. increment
2. identity
3. sequence
4. hilo
5. seqhilo

**要求是字符串时**

guid(sqlserver mysql)
uuid

**用户自定义**

assigned

### 2.2.2 property
property表示普通的属性
- name:属性的名字
- column：字段的名字
- length:字段的长度

# 3. 三态
瞬时态，持久态，游离态

# 4. 映射关系
## 4.1 特殊操作
### 4.1.1 级联操作
**在操作一方时，可以选择级联操作,有四种可选**：all、save-update、none(默认)、delete:

**all**:有所有操作的级联。1方做添加/修改：先自动的将多方插入到数据库。1方做删除：先将多方数据删除，然后删除1方

**save-update**:1方做添加/修改会进行级联

**delete**:1方做删除时才会做级联操作
### 4.1.2 inverse(反转)
一般默认为`false`,但我们可以将**一方**的设置为`true`，让双方中的**多方**来维护数据库。

如果不设置，在一方增删时，会联动的对多方产生冗余操作

## 4.2 单向
单向时只需要在其中一方添加配置，一方或者多方，视需要完成的操作而选择

### 4.2.1 一对N
**在一方的配置文件增加配置**

```xml
<hibernate-mapping package="com.bjsxt.ly.vo">
    <class name="Dept" table="t_dept">
        <id name="deptno">
            <generator class="uuid"></generator>
        </id>
        <property name="dname"></property>
        <property name="loc"></property>
        <!-- 配置单向的一对多 -->
        <set name="emps" cascade="delete">
            <key column="deptno"></key>
            <one-to-many class="Emp" />
        </set>
    </class>
</hibernate-mapping>
```
### 4.2.2 N对一
**在多方的配置文件增加设置**

```xml
<hibernate-mapping package="com.bjsxt.ly.vo">
    <class name="Emp" table="t_emp">
        <id name="empno">
            <generator class="uuid"></generator>
        </id>
        <property name="ename"></property>
        <property name="job"></property>
        <!-- 配置多对一 -->
        <many-to-one name="dept" class="Dept" column="deptno"></many-to-one>
    </class>
</hibernate-mapping>
```
## 4.3 双向
inverse
### 4.3.1 一对N
**一方和多方的配置文件都要设置**
对于双向而言，1对N和N对1，性质一样
**一方的配置：**

```xml
<hibernate-mapping package="com.bjsxt.ly.vo">
    <class name="Dept" table="t_dept">
        <id name="deptno">
            <generator class="uuid"></generator>
        </id>
        <property name="dname"></property>
        <property name="loc"></property>
        <!-- 配置双向的一对多-->
        <set name="emps" cascade="all" inverse="true">
            <key column="deptno"></key>
            <one-to-many class="Emp" />
        </set>
    </class>
</hibernate-mapping>
```
**多方的配置：**

```xml
<hibernate-mapping package="com.bjsxt.ly.vo">
    <class name="Emp" table="t_emp">
        <id name="empno">
            <generator class="uuid"></generator>
        </id>
        <property name="ename"></property>
        <property name="job"></property>
        <!-- 配置双向的一对多 -->
        <many-to-one name="dept" class="Dept" column="deptno"></many-to-one>
    </class>
</hibernate-mapping>
```
### 4.3.2 1对1
需要在双方两个类都配置对应的类的属性字段
1对1就是一个特殊的N对1.

#### 外键方式
**Address类的配置文件**

```xml
<hibernate-mapping package="com.bjsxt.ly.vo">
    <class name="Address" table="t_address">
        <id name="aid">
            <generator class="uuid"></generator>
        </id>
        <property name="aname"></property>
        <!-- 一对一 -->
        <one-to-one name="company" class="Company" cascade="all" property-ref="address"></one-to-one>
    </class>
</hibernate-mapping>
```
**Company类的配置文件**

```xml
<hibernate-mapping package="com.bjsxt.ly.vo">
    <class name="Company" table="t_company">
        <id name="cid">
            <generator class="uuid"></generator>
        </id>
        <property name="cname"></property>
        <!-- 写一个映射 -->
        <many-to-one name="address" class="Address" column="aid" unique="true" cascade="all"></many-to-one>
    </class>
</hibernate-mapping>
```
#### 主键方式

主键方式的类基本一样，只是配置文件有所区别
Address类的配置文件

```xml
<hibernate-mapping package="com.bjsxt.ly.vo">
    <class name="Address" table="t_address">
        <id name="id">
            <generator class="uuid"></generator>
        </id>
        <property name="aname"></property>
        <!-- 一对一 -->
        <one-to-one name="company" class="Company" cascade="all"></one-to-one>
    </class>
</hibernate-mapping>
```
**Company类的配置文件**

```xml
<hibernate-mapping package="com.bjsxt.ly.vo">
    <class name="Company" table="t_company">
        <id name="id">
            <!-- 这里标识要用外键 -->
            <generator class="foreign">
                <param name="property">address</param>
            </generator>
        </id>
        <property name="cname"></property>
        <!-- 写一个映射 -->
        <one-to-one name="address" class="Address" cascade="all" constrained="true"></one-to-one>
    </class>
</hibernate-mapping>
```

### 4.3.3 N对N
对于N对N，除了在每个N的对着的类加入`Set<>`的字段外，其还要在配置文件中配置
对于Hero和User这种多对多的关系
**User类：**

```xml
<hibernate-mapping package="com.bjsxt.ly.vo">
    <class name="User" table="t_user">
        <id name="id">
            <generator class="uuid"></generator>
        </id>
        <property name="uname"></property>
        <property name="realname"></property>
        <!-- 多对多配置 -->
        <set name="heros" cascade="save-update" table="t_user_hero">
            <key column="uid"></key>
            <many-to-many column="hid" class="Hero"></many-to-many>
        </set>
    </class>
</hibernate-mapping>
```

**Hero类：**

```xml
<hibernate-mapping package="com.bjsxt.ly.vo">
    <class name="Hero" table="t_hero">
        <id name="id">
            <generator class="uuid"></generator>
        </id>
        <property name="hname"></property>
        <property name="title"></property>
        <!-- 多对多关联映射 -->
        <set name="users" cascade="save-update" table="t_user_hero" inverse="true">
            <key column="hid"></key>
            <many-to-many column="uid" class="User"></many-to-many>
        </set>

    </class>
</hibernate-mapping>
```

## 4.4 总结

对于出现**N**的关系映射，对应的1方或者多方，都要在java类的属性中增加对应的`Set<>`的字段

对于出现**1**的关系映射，对应的1方或者多方，都要在java类的属性中增加对应的1所属方的类做的属性字段
对于Dept和Emp这种1对N的关系

**Dept:**

```java
private Set<Emp> emps = new HashSet<>();
...
```

**Emp:**

```java
private Dept dept;
...
```

其对应的配置文件

**Dept:**

```xml
<!-- 与Emp的1对多关系 -->
<set name="emps" cascade="all" inverse="true">
    <key column="deptid"></key>
    <one-to-many class="Emp" />
</set>
```

**Emp:**

```xml
<!-- 配置与Dept的多对一关系 -->
<many-to-one name="dept" class="Dept" column="deptid"></many-to-one>
```
**N对N是特殊的1对N**

# 5. HQL


HQL和SQL很类似，但不一样。
HQL是面向对象的，from后跟的也是类名而不是表名。而且可以直接通过`.`这种java面向对象的方式把值求出来，这点在传统SQL使用多表查询时有很大优势。

## 5.1 基本步骤

1. 获取`Hibernate Session`对象
2. 编写`HQL`语句
3. 以`HQL`语句作为参数，调用`Session`的`createQuery()`方法创建查询对象
4. 如果`HQL`语句包含参数，则调用`Query`的`set`方法为参数赋值
5. 调用`Query`对象的`list()`或`uniqueResult()`方法返回查询结果列表（持久化实体集）

**给HQL占位符赋值时，是从0开始的**
# 6. 条件查询（Criteria）

## 6.1 基本步骤

1. 获得Hibernate的`Session`对象
2. 以`Session`对象创建`Criterion`对象
3. 使用`Restrictions`的静态方法创建`Criterion`查询条件
4. 向`Criteria`查询中添加`Criterion`查询条件
5. 执行`Criteria`的`list()`或`uniqueResult()`方法返回结果

```java
public static void main(String args){
	//获取session对象
	Session session = HibernateUtil.openSession();
	//创建criteria对象
	Criteria criteria = session.createCriteria(User.class);
	//创建查询条件
	criteria.add(Restrictions.between("age",10,20));
	//添加查询条件
	criteria.add(Restrictions.like("id","23tgei0g")).add(Restrictions.in("pwd",new String[]{"1","2"}))
	//得到查询结果
	List<User> list = criteria.list();
}
```

# 7. 缓存

## 7.1 特定操作与缓存的关系
### 7.1.1 查询
- **get:**
会直接去查询数据：首先去`session`中查询，然后再去****数据库**查询
找到后会直接将结果放在**一级缓存**；找不到，则会返回`null`
- **load:**
首先判断当前查询的对象**是否会被使用**：
**不使用**，延迟查询；
**使用**，开始查询**一级缓存**，然后**数据库**查询，如果查询到会保存在**缓存**等待以后使用，查询不到返回`null`
- **list:**
直接去**数据库**查询，会将查询结果放入**缓存**
- **iterate:**
直接去数据库查询出所有数据的`ID`,当使用本条数据的时候根据`ID`再去查询（先从**缓存**查询，再去****数据库**查询，查询到的数据会保存在**数据库**）

### 7.1.2 清空缓存
- **clear:**
清空当前缓存
- **evict:**
清空指定对象的缓存信息
- **flush:**
强制将缓存中的数据同步到数据库

## 7.2 二级缓存
### 7.2 开启二级缓存
1. 拷贝`lib\optional\ehcache`并build path
2. 去拷贝`ehcache`的配置文件`project\etc\ehcache.xml`
3. 修改主配置文件，添加如下：

```xml
<!-- 二级缓存 -->
<!-- 第一条直接去property文件复制过来 -->
<property name="hibernate.cache.use_second_level_cache">true</property>
<!--  -->
<property name="hibernate.cache.region.factory_class">org.hibernate.cache.ehcache.EhCacheRegionFactory</property>
```
# 8. 懒加载
# 9. 批处理
# 10. 死锁
# 11. annotation
使用annotation需要额外增加3个jar包：`hibernate-annotatons.jar`,`ejb3-persistence.jar`和`hibernate-commons-annotations.jar`

## 11.1 常见的注解
- @Entity:将一个类声明为一个实体`bean`(即一个持久化POJO类)
- @Table:声明该实体bean映射指定的表(table)目录(catalog)和schema的名字
- @Id:声明了该实体bean的标识属性
- @Generated:声明了主键的生成策略
- @Transient:声明的属性不会跟数据库关联
7