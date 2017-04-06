[TOC]
# 1. 基本流程
- 拷贝`Mybatis`的主配置文件
- 拷贝`lib`中关于代理的`jar`包
- 拷贝关于日志文件的`jar`包
- 拷贝关于数据库的`jar`包
- 创建并拷贝`log4j`的配置文件

# 2. 使用步骤

1. 开始读取配置文件
2. 解析配置文件
3. 创建`session`工厂
4. 获取数据库的链接
5. 开始检索数据
6. 关闭`session`

```java
public class TestMyBatis {
	public static void main(String[] args) throws IOException {
		//开始读取配置文件
		InputStream inputStream = Resources.getResourceAsStream("mybatis.cfg.xml");
		//开始解析配置文件
		SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
		//创建session工厂
		SqlSessionFactory factory = builder.build(inputStream);
		//获取数据库的链接
		SqlSession session = factory.openSession();
		//开始检索数据
		User user = session.selectOne("selectById");
		System.out.println(user);
		//关闭session
		session.close();
	}
}
```

# 3. select语句
## 3.1 selectOne
## 3.2 selectList
## 3.3 selectMap
## 3.4 参数传递
- **普通参数**：用来传递只有一个参数
- **对象参数**：用来传递，参数都是对象的属性(类的字段)
- **map参数**：用来传递互相没有关系的参数
# 4. resultType和resultMap
**resultType**:当**不需要**传入参数且类的字段名和表的列名**一一对应**进行检索时，选择`resultType`
**resultMap**:当需要**传入参数**或者需要专门在`resultMap`**指定**类的字段名和表列名的映射关系而进行检索时，选择`resultMap`

```xml
<resultMap>
	<resultMap id="UserResultMap" type="User">
		<id property="id" column="id" />
		<!-- 对于已经一一对应的字段名可以省略-->
		<result property="uname" column="uname" />
		<result property="createtime" column="createtime" />
		<result property="phoneNo" column="phonenum" />
	</resultMap>
</resultMap>
```
# 5. SQL语句
```xml
<select id="selectLogin1" parameterType="String" resultMap="UserResultMap">
		select * from t_user where uname = #{uname}
	</select>

	<select id="selectLogin2" parameterType="User" resultMap="UserResultMap">
		select * from t_user where uname = #{uname} and realname = #{realname}
	</select>

	<select id="selectLogin3" parameterType="Map" resultMap="UserResultMap">
		select * from t_user where uname = #{uname} and realname = #{realname}
	</select>

	<select id="selectPage1" parameterType="Map" resultMap="UserResultMap">
		select * from t_user limit #{rowStart},#{size}
	</select>
	<select id="selectPage2" resultMap="UserResultMap">
		select * from t_user
	</select>
	<select id="selectCount" resultType="int">
		select count(*) from t_user
	</select>
	<select id="selectDy" parameterType="Map" resultType="String">
		select ${column} from ${tablename} order by phonenum ${ordertype}
	</select>
	<select id="selectLike1" parameterType="String" resultMap="UserResultMap">
		select * from t_user where uname like concat('%',#{uname},'%')
	</select>
	<select id="selectLike2" parameterType="Map" resultMap="UserResultMap">
		select * from t_user where uname like '%${uname}%'
	</select>
```
## `SQL`语句从传入参数取值有两种`#{}`和`${}`

**`#{}`**:`#`相当于对数据加上**双引号**

**`${}`**:`$`相当于直接显示动态的数据，不加**引号**

它们的区别：

1. `#`将传入的数据都当成一个字符串，会对自动传入的数据加一个双引号。如：`order by #user_id#`，如果传入的值是111,那么解析成sql时的值为order by "111", 如果传入的值是id，则解析成的sql为order by "id".　
2. `$`将传入的数据直接显示生成在sql中。如：`order by $user_id$`，如果传入的值是111,那么解析成sql时的值为order by user_id,  如果传入的值是id，则解析成的sql为order by id.　
3. `#`方式能够很大程度防止sql注入。　
4. `$`方式无法防止Sql注入。
5. `$`方式一般用于传入数据库对象，例如传入表名.　
6. 一般能用`#`的就别用`$`.

`MyBatis`排序时使用`order by`动态参数时需要注意，用`$`而不是`#`

# 6. 模糊查询
# 7. 关系映射
## 7.1 o2m
## 7.2 m2o
## 7.3 m2m
对于Dept和Emp的m2m关系
### collection
```xml
<resultMap type="Dept" id="deptMap">
	<id property="deptno" column="deptno"/>
	<result property="dname" column="dname" />
	<!-- 配置一对多关系 -->
	<collection property="emps" javaType="java.util.ArrayList" ofType="Emp" select="com.bjsxt.arnold.dao.EmpDao.selectEmpByDept" column="deptno"></collection>
</resultMap>
```
# 8. 分页
# 9. 事务