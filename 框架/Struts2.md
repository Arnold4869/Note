[TOC]
# 1. struts2的配置
1. 创建一个web项目
2. 解压struts2压缩包，将apps文件夹下的struts2-blank.war解压
3. 创建一个web项目，在java1.6版本之上时，记住勾选创建web.xml文件
4. 拷贝blank项目下WEB-INF\lib下的所有jar包到创建的项目下的lib目录
5. 拷贝web.xml中的配置信息
6. 拷贝struts2的主配置文件到src目录
    `struts-blank\WEB-INF\classes`下的`struts.xml`和`log4j2.xml`
7. 删除`struts.xml`下所有配置信息，只保留`package`标签
8. 在`package`中创建一个`action`的标签，对应属性有`name`和`class`
    `<action name="test" class="com.bjsxt.arnold.web.Test"></action>`
9. 将项目部署到服务器，启动成功

# 2. 创建action的三种方式
1. **普通java类**
2. **实现接口**
实现`Action`接口
3. **继承类**
继承`ActionSupport`类


# 3. struts.xml详解
## 3.1 constant配置
### 3.1.1 DMI动态方法调用
` <constant name="struts.enable.DynamicMethodInvocation" value="true" />`
**默认是false**
启用后，可以直接用`!`来直接调用非`execute()`方法

```html
<ol>
    <li><a href="method">DMI</a></li>//默认调用execute()方法
    <li><a href="method!add">DMI_add</a></li>//调用add()方法
    <li><a href="method!delete">DMI_delete</a></li>//调用delete()方法
</ol>

```

### 3.1.2 devMode开发模式
可以在不重启服务器的情况下更新一些改动

### 3.1.3 i18n国际化编码
可以在`struts.xml`中设置
```xml
<constant name="struts.i18n.encoding" value="UTF-8" />
```
**对于get方式，还需要在tomcat中配置`useBodyEncodingForURI="true"`**
## 3.2 package配置
### 3.2.1 name属性
**用来区分模块，还可以通过名字让别的包来继承当钱包**

### 3.2.2 namespace属性
保护当前模块下的路径安全

### 3.2.3 extends属性
继承别的包，获取别的包中所有的功能（公共结果集，拦截器，拦截器栈），一般直接或间接的继承自*struts-default*

### 3.2.4 abstract属性
抽象的包

## 3.3 action配置
### 3.3.1 name属性
**当前action的访问路径，受命名空间的保护**

### 3.3.2 class属性
当前action对象，就是需要通过反射创建的action对象

### 3.3.3 method属性
需要执行的方法，因为DMI的原因很少使用

## 4. result
### 4.1 name属性
### 4.2 type属性
默认有11种，常用5种

```xml
	<!-- dispatcher(默认),请求转发到jsp -->
	<result name="disp">index.jsp</result>
	<!-- chain,请求转发到action -->
	<result name="chain" type="chain">
		<param name="namespace">/teacher</param>
		<param name="actionName">tea</param>
		<param name="method">chain</param>
	</result>
	<!-- redirect,重定向到jsp -->
	<result name="redi" type="redirect">/teacher/index.jsp?uname=zhaomin&amp;realname=${realname}</result>
	<!-- redirectAction,重定向到action -->
	<result name="ra" type="redirectAction">
		<param name="namespace">/teacher</param>
		<param name="actionName">tea</param>
		<param name="method">chain</param>
		<param name="uname">yangguo</param>
		<param name="nickname">${realname}</param>
	</result>
```
#### dispatcher(默认)
请求转发到jsp

#### chain
请求转发到action

#### redirect
重定向到别的jsp

#### redirectAction
重定向到别的action

#### stream
ajax、上传下载



## 5. include
通过设置**include**可以包含其他的*struts.xml*,方便分组做项目

# 4. action端获取表单提交数据
## 4.1 属性驱动
1. 需要在前端表单获取数据的时候，设置为`变量名.属性`
2. 在对应类中设置`private User abcd;`

## 4.2 模型驱动
1. 在对应类实现`ModelDriven<User>`**接口**，且实现的方法`getModel()`返回值改为`return this.user`类似的形式
2. 在对应类设置`private User abcd = new User();`

# 5. 作用域
## 5.1 基本流程
1. 每次当我们发送请求是，struts的过滤器都会帮我们创建一个*ActionContext*对象
2. *ActionContext*里面包含了*request*对象，而可以由它导出*session*对象和*servletContext(application)对象*
3. *ActionContext*对象在过滤器中创建，我们在*action*中使用
4. 这之间通过在一个公有区域存放，利用*ThreadLocal*设置和获取

```java
public static ThreadLocal<ActionContext> actionContext = new ThreadLocal<ActionContext>();
    public static void setContext(ActionContext context) {
        actionContext.set(context);
    }

    public static ActionContext getContext() {
        return actionContext.get();
    }
```
**ThreadLocal**类似于`Map<currentThread,T>`，通过线程来当做Map中的*key*,所以设置和获取值时都不用写*key*


## 5.2 普通方法
### 5.2.1 Ognl
**准备：**

1. 在项目中导入`javassist-3.11.0.GA.jar`和`ognl-3.0.6.jar`两个jar包
2. 在页面引入struts标签库

```html
<%@ taglib uri="/struts-tags" prefix="s"%>
```
具体操作
**在action(即后台java类)中**

```java
//例如从context中获取
Ognl.getValue("#uname", context, root);
//从root中获取
Ognl.getValue("uname", context, root);
```
1. **express:**

	使用**`#`**从*context*取值，普通字符从root取值

2. **context:(三个作用域,paramters,attr,action)**

	`private Map<String , Object> context;`

3. **root:**

	action(valueStack)

**在前台，需要先导入标签库**

```html
	<h1>User Index</h1>
	<s:property value="#bjsxt" />
	<br />
	<s:property value="#request.bjsxt" />
	--${requestScope.bjsxt }
	<br />
	<s:property value="#session.bjsxt" />
	--${sessionScope.bjsxt }
	<br />
	<s:property value="#application.bjsxt" />
	--${applicationScope.bjsxt }
	<br />
	<s:property value="#parameters.uname" />
	--${param.uname }
	<br />
	<s:property value="#attr.bjsxt" />-1111111111111111111111111-${bjsxt }
	<br />
	<s:property value="#action.uname" />
	<br /> ${uname }
	<br/>
	<s:property value="uname"/>

	<s:debug></s:debug>
```
### 5.2.2 EL表达式

# 6. ajax

## 6.1 struts1

```java
public String testAjaxS1() throws IOException {
	System.out.println("UserAction.testAjaxS1([" + uname + "][" + realname + "])");

	HttpServletResponse response = ServletActionContext.getResponse();
	response.setContentType("text/html;charset=utf-8");
	PrintWriter out = response.getWriter();
	out.println("Hello World!  This is an AJAX response from a Struts Action.来自于北京尚学堂");
	out.flush();
	return this.NONE;
}

```
## 6.2 struts2

配置文件：

```xml
<action name="text-result" class="actions.TextResult">
    <result type="stream">
        <param name="contentType">text/html;charset=utf-8</param>
        <param name="inputName">inputStream</param>
    </result>
</action>
```

后台代码：

```java
public String testAjaxS2() throws IOException {
	System.out.println("UserAction.testAjaxS1([" + uname + "][" + realname + "])");
	inputStream = new ByteArrayInputStream("Hello World! This is a text string response from a Struts 2 Action.来自于北京尚学堂".getBytes("UTF-8"));
	return "ajax";
}
```
# 7. 文件的上传下载
## 7.1 上传
- 需要将表单的`method`设置为**post**,标签里也要设置*enctype*且值为`multipart/form-data`
- 在对应的*action*中设置**成员变量**`private File upload;private String uploadContentType;private String uploadFileName`和对应的**Set/Get**方法

## 文件大小的限制
通过拦截器实现文件大小的设置

```xml
<action name="user" class="com.bjsxt.ly.user.UserAction">
	<interceptor-ref name="fileUpload">
		<!-- 单位为字节 -->
		<param name="maximumSize">2097152</param>
	</interceptor-ref>
	<!-- 如果需要使用文件上传拦截器来过滤文件大小，或者过滤文件的类型，则必须显
	示的配置引用Struts默认的拦截器栈：defaultStack，而且fileUpload拦截器必须配置
	在defaultStack拦截器栈之前 -->
	<interceptor-ref name="defaultStack"></interceptor-ref>
	<result>index.jsp</result>
</action>
```
如果需要使用文件上传拦截器来过滤文件大小，或者过滤文件的类型，则必须显示的配置引用Struts默认的拦截器栈：defaultStack，而且fileUpload拦截器必须配置在defaultStack拦截器栈之前
### 全局限制
### 局部限制
## 7.2 下载
**后台代码**

```java
public String download() throws FileNotFoundException, UnsupportedEncodingException {
	//文件资源
	inputStream = new FileInputStream("D://" + fileName);
	//文件的乱码问题
	fileName = new String(fileName.getBytes("utf-8"), "ISO-8859-1");
	return "download";
}
```
**struts.xml配置**

```xml
<action name="book" class="com.bjsxt.ly.book.BookAction">
	<result name="download" type="stream">
		<!-- 指定被下载文件的类型 -->
		<param name="contentType">application/x-download</param>
		<!-- 指定被下载文件的入口输入流 -->
		<param name="inputName">inputStream</param>
		<!-- 指定下载文件名 -->
		<param name="contentDisposition">attachment;filename="${fileName}"</param>
		<!-- 指定下载文件时的缓冲大小 -->
		<param name="bufferSize">4096</param>
	</result>
</action>
```

# 8. 拦截器
## defaultStack
## 在线支付
## 自定义拦截器
