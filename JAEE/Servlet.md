[TOC]

# 1. 基于Myeclipse部署
## 安装配置tomcat服务器
1. 下载解压tomcat压缩包
2. 将解压的文件夹放在任意非特殊字符的目录
3. 将目录的路径配置在一个`TOMCAT_HOME`的路径

## 1.1 在Myeclipse配置绑定tomcat服务器

1. 现在`windows-Preference-Myeclipse-Server-Tomcat`下选择一个对应版本的选项。例如`7.x`
2. 选择**Enable**,再在`Tomcat home directory`选择`Tomcat`的安装路径
3. 在主界面找到`Deploy Myeclipse JavaEE Project to server`
4. 将要部署的项目部署

# 2. 基本方法
对于所有继承servlet类的实现类，都需要至少重写以下三个方法之一
1. service()：可以处理`get`和`post`请求
2. doGet():可以处理`get`方式的请求，不能处理`post`方式处理的请求
3. doPost():可以处理`post`方式的请求，不能处理`get`方式的请求

注意：**请求来时，服务器会优先执行`service`方法，如果没有`service`才会执行对应的`post`或者`get`方法**

# 3. 编码

## 3.1 请求时编码
### 3.1.1 post方式
只需要在servlet类中添加如下代码

```java
req.setCharacterEncoding("utf-8");
```
### 3.1.2 get方式

需要先在`server.xml`中的`Connector`标签中增加属性`useBodyEncodingForURI="true"`，然后再添加如下类似代码

```java
req.setCharacterEncoding("utf-8");
```

## 3.2 响应时编码

可以采用3种，一般采用第二种，最简单
**第一种：**

```java
//需要在resp.getWriter()之前进行设置
resp.setCharacterEncoding("utf-8");
//通知浏览器服务器发送的数据格式是text/html,并要求浏览器用utf-8进行编码
resp.setHeader("ContentType","text/html;charset=utf-8");
```
**第二种：**

```java
//通知浏览器发送的格式是text/html，服务器采用utf-8进行编码，并要求浏览器使用utf-8解码
resp.setContentType("text/html;charset=utf-8");
```
第二种等同于`resp.setHeader("ContentType","text/html;charset=utf-8");`，并会覆盖`resp.setCharacterEncoding("utf-8")`

**第三种：**

```java
resp.setCharacterEncoding("utf-8");
resp.getWriter.println("<meta http-equiv='Content-Type' content='text/html;charset=utf-8'>");
```


# 4. 常用方法

## 4.1 设置响应头

- addHeader(name,value):增加响应头，一个键可以有多个值；
- setHeader(name,value):增加响应头，一个键只能有一个值；
- resp.setHeader("content-type","text/html;charset=utf-8");
- resp.setHeader("content-type","text/plain;charset=utf-8");
- resp.setHeader("content-type","text/xml;charset=utf-8");

## 4.2 设置响应实体
`response.getWriter().write("实体内容");`

# 5. 获取请求信息
## 5.1 请求行信息
- getMethod():获取请求方式
- getRequestURL(): 获取URL信息
- getRequestURI(): 获取URI信息
- getQueryString(): 获取用户请求信息
- req.getScheme(): 获取协议信息

## 5.2 请求头信息
- getHeader("键名")；
- getHeaderNames():返回所有请求头的**键名的枚举集合**

```java
//使用getHeaderNames返回请求头的name枚举对象
Enumaration en = req.getHeaderNames();
//遍历枚举对象
while(en.hasMoreElements()){
	String name = (String)en.nextElement();
	String value = req.getHeader(name);
	System.out.println(name+":"+value);
}
```
## 5.3 用户请求信息
- getParameter("键名")：获取指定键名的用户信息
- getParameterValues("键名")：获取同名不同值的用户信息
- getParameterNames():获取所有用户请求信息键名的枚举对象集合

```java
Enumeration em = req.getParameterNames();
while(em.hasMoreElements()){
	String name = (String)em.nextElement();
	if("fav".equals(name)){
		String[] str = req.getParameterValues("fav");
		//遍历str
		for(int i = 0; i < str.length; i++){
			System.out.println("fav:" + str[i]);
		}
	}else{
		String value = req.getParameter(name);
		System.out.println(name + "=" + value);
	}
}
```
# 6. 请求转发
定义：**一个请求需要多个`servlet`协同进行处理，这样就需要一次请求内的多个`servlet`使用同一个`request`对象**

## 6.1 使用方式
相互调用，将request对象作为实参进行传递
在servlet中使用：

```java
req.getRequestDispatcher("要转发到的servlet的URL-Pattern").forward(req,resp);
```

## 6.2 特点
请求的URL信息没有变化
一次请求，多个`servlet`协同处理，共享一个请求对象

## 6.3 request对象的作用域
一次请求的多个servlet
作用域的使用：

```java
setAttribute(key,value);//向request对象中添加自定义数据
getAttribute(key);//取出自定义的数据
removeAttribute(key);//删除自定义数据
```
## 6.4 requeset的生命周期
从请求开始到请求结束

# 7. 重定向
对于不能处理的请求，可以转发给可用的程序

## 7.1 使用重定向

```java
resp.sendRedirect("URL信息");//重定向项目外资源
resp.sendRedirect("url-pattern");//重定向自己
```

## 7.2 特点
- 地址栏URL信息改变
- 两次请求

# 8. Cookie
Http是无状态的，但是不同请求之间是需要共享数据的。
使用Cookie可以在一次登录后，在浏览器浏览其它页面时，自动发送带有`Cookie`的信息，告诉页面是谁在访问，不用再重复登录

## 8.1 使用
1. 创建Cookie对象
2. 设置Cookie
3. 响应Cookie信息到客户端

```java
//创建Cookie对象
Cookie ck = new Cookie(key,value);
//设置Cookie
setMaxAge(有效时间，单位秒);
setPath(URL);
//将Cookie对象响应到客户端
resp.addCookie(ck);
```

**Cookie默认情况下保存在浏览器的运存中，关闭浏览器就自动销毁**
## 8.2 获取Cookie

```java
//获取Cookie信息对象数组
Cookie[] cks = req.getCookies();
//遍历数组，得到Cookie信息
if(cks != null){
	for(Cookie ck : cks){
		System.out.println("ck");//可以是其它别的操作啦
	}
}
```

# 9. session
用来解决不同业务处理`servlet`之间的数据库共享，如果没有session，则每次都要查询数据库，太麻烦了。而`session`可以做到不同`servlet`之间数据共享

## 9.1 创建或获取session对象

```java
HttpSession hs = req.getSession();
```

**如果请求中没有SessionID,这个语句则会返回一个新的`SessionID`,如果请求中含有`SessionID`，则会把这个`SessionID`返回**
## 9.2 使用session对象

```java
hs.setAttribute(key,value);//向作用域中添加数据
hs.getAttribute(key);//得到数据
```

## 9.3 设置session对象
`SessionID`的默认有效期为浏览器不关闭都有效

```java
//设置有效期
hs.setMaxInactiveInterval(3600*2);//设置两个小时
//强制失效
hs.invalidate();
```
## 9.4 生命周期和作用域
生命周期：一次会话
作用域：浏览器不关闭，`session`不失效，在项目范围内任意位置都可以获得相同的`SessionID`。