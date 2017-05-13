[TOC]

## 1. 拦截器 过滤器 监听器

### 过滤器

Servlet 中的过滤器 Filter 是实现了 `javax.servlet.Filter` 接口的服务器端程序，主要用途是过滤字符编码，做一些业务判断。**它随着 web 应用启动而启动，只初始化一次，以后就可以拦截相关请求，只有当 web 应用停止或重新部署的时候才销毁。需要在 `web.xml` 中配置 `<filter>`**


```java
public class MyCharsetFilter implements Filter {
    private FilterConfig config = null;

    public void destroy() {
       System.out.println("MyCharsetFilter准备销毁...");
   }


   public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws IOException, ServletException {
         // 强制类型转换,ServletRequest需要转换成HttpServletRequest 才能更好处理
       HttpServletRequest request = (HttpServletRequest)req;
       HttpServletResponse response = (HttpServletResponse)resp;
       chain.doFilter(request, response);
   }
     //初始化时会被调用
   public void init(FilterConfig arg0) throws ServletException {
       this.config = arg0;
       System.out.println("MyCharsetFilter初始化...");
   }
}
```


### 监听器

它是实现了 `javax.servlet.ServletContextListener` 接口的服务器端程序，**它也是随 web 应用的启动而启动，只初始化一次，随 web 应用的停止而销毁。主要作用是： 做一些初始化的内容添加工作、设置一些基本的内容、比如一些参数或者是一些固定的对象等等。**需要在 web.xml 中配置 `<listener>`


```java
public class MyServletContextListener implements ServletContextListener {
     // 应用监听器的销毁方法
   public void contextDestroyed(ServletContextEvent event) {
       ServletContext sc = event.getServletContext();
         // 在整个web应用销毁之前调用，将所有应用空间所设置的内容清空
       sc.removeAttribute("dataSource");
       System.out.println("销毁工作完成...");
   }

   public void contextInitialized(ServletContextEvent event) {
       System.out.println("应用监听器初始化工作完成...");
       System.out.println("已经创建DataSource...");
   }
}

```

### 拦截器

拦截器是在**面向切面**编程中应用的，就是在你的 service 或者一个方法前调用一个方法，或者在方法后调用一个方法。是基于 Java 的反射机制。拦截器不是在 web.xml，比如 struts 在 struts.xml 中配置，

```java
public Object invoke(Object proxy, Method method, Object[] args) throws Throwable
{
    Object result = null;
    System.out.println("before invoke method :" + method.getName());
    result = method.invoke(this.targetObj, args);
    System.out.println("after invoke method : " + method.getName());
    return result;
}
```

### 总结
1. 过滤器：所谓过滤器顾名思义是用来过滤的，在 Java web 中，你传入的 request,response 提前过滤掉一些信息，或者提前设置一些参数，然后再传入 servlet 或者 struts 的 action 进行业务逻辑，比如过滤掉非法 url （不是login.do的地址请求，如果用户没有登陆都过滤掉）,或者在传入 servlet 或者 struts 的 action 前统一设置字符集，或者去除掉一些非法字符。filter 流程是线性的， url 传来之后，检查之后，可保持原来的流程继续向下执行，被下一个 filter, servlet 接收等.

2. 监听器：这个东西在**c/s**模式里面经常用到，他会对特定的事件产生产生一个处理。监听在很多模式下用到。比如说观察者模式，就是一个监听来的。又比如 struts 可以用监听来启动。Servlet 监听器用于监听一些重要事件的发生，监听器对象可以在事情发生前、发生后可以做一些必要的处理。

3. java 的拦截器 主要是用在插件上，扩展件上比如 hivernate spring struts2等 有点类似面向切片的技术，在用之前先要在配置文件即xml文件里声明一段的那个东西。