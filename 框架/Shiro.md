[TOC]

# 1. 主要概念
- **Subject**：主体，可以理解为用户，可以通过调用`isAuthentiacion`方法，获取当前用户是否认证通过
- **Security Manager**:shiro核心，管理认证、授权、会话管理、`reaml`和session Dao
- **Authenticator**:认证器
- **Authorizer**:授权器
- **Realm**:领域。可自定义`Realm`，认证、授权的核心业务逻辑在这里实现
- **Cache Manager**:缓存管理器，可以和缓存的框架结合
- **Cryptography**:密码组件，提供常用的编码、解码、加密算法

# 2. 主要流程
1. 构建`Factory`工厂
2. 构建`SecurityManager`添加至`SecurityUtils`
3. 获取**主体**
4. 登录（使用`AuthenticationToken`）
5. 验证

# 3. 基本代码实现

```java
package com.bjsxt.shiro;

import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.config.IniSecurityManagerFactory;
import org.apache.shiro.mgt.SecurityManager;
import org.apache.shiro.subject.Subject;
import org.apache.shiro.util.Factory;
import org.junit.Test;
public class HelloShiroTest {

    public void testAuthentication(){
        //1.实例化 工厂
        Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:shiro.ini");
        //2.构建SecurityManager
        SecurityManager manager = factory.getInstance();
        //3.将SecurityManager 添加至SecurityUtils
        SecurityUtils.setSecurityManager(manager);
        //4.获取主体
        Subject subject = SecurityUtils.getSubject();

        //模拟用户输入的用户名和密码
        AuthenticationToken token = new UsernamePasswordToken("bjsxt", "111111");

        //5.登录
        subject.login(token);

        /**
         * 6.验证
         * 如果用户名输错 抛出 UnknownAccountException
         * 如果密码错误 抛出 IncorrectCredentialsException
         */
        boolean isAuthenticated = subject.isAuthenticated();

        //打印结果
        System.out.println("是否认证通过："+isAuthenticated);
    }
}
```
# 4. Shiro内部调用过程
`DelegatingSubject.login()`——>
`DefaultSecurityManger.login()`——>
`AuthenticationSecurityManager. authenticate()`——>
`AbstractAuthenticationtor. authenticate ()`——>
`ModularRealmAuthenticationtor. doAuthenticate()`——>
`ModularRealmAuthenticationtor. doSingleRealmAuthentication()`——>`AuthticatingRealm. getAuthenticationInfo()`

# 5. 自定义Realm

```java
package com.bjsxt.shiro.realm;

import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.IncorrectCredentialsException;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.authc.UnknownAccountException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.realm.Realm;

/**
 * 自定义realm 实现realm接口 或者继承该接口中的实现类
 * @author zhiduo
 *
 */
public class MyRealm1 implements Realm {

    @Override
    public String getName() {
        return "myrealm1";
    }

    @Override
    public boolean supports(AuthenticationToken token) {
        //只提供用户名、密码的验证
        return token instanceof UsernamePasswordToken;
    }

    /**
     * 自定义认证的过程
     */
    @Override
    public AuthenticationInfo getAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        System.out.println("First realm");
        //得到form表单提交的用户名和密码 Principal认证唯一标识 比如用户名、邮箱、身份证、手机号
        String username = (String)token.getPrincipal();
        //得到密码 凭证：密码
        String password = new String((char[])token.getCredentials());

        //自定义认证逻辑
        //假设从数据库中查询的用户名、密码
        String usernameDb = "bjsxt";
        String passwordDb = "111111";

        if(!usernameDb.equals(username)){
            throw new UnknownAccountException("该用户不存在");
        }
        if(!passwordDb.equals(password)){
            throw new IncorrectCredentialsException("请输入正确的密码");
        }
        return new SimpleAuthenticationInfo(username, password, getName());
    }

}

```

```java
package com.bjsxt.shiro.realm;

import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.IncorrectCredentialsException;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.authc.UnknownAccountException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.realm.Realm;

/**
 * 自定义realm 实现realm接口 或者继承该接口中的实现类
 * @author zhiduo
 *
 */
public class MyRealm2 implements Realm {

    @Override
    public String getName() {
        return "myrealm1";
    }

    @Override
    public boolean supports(AuthenticationToken token) {
        //只提供用户名、密码的验证
        return token instanceof UsernamePasswordToken;
    }

    /**
     * 自定义认证的过程
     */
    @Override
    public AuthenticationInfo getAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        System.out.println("Second realm");
        //得到form表单提交的用户名和密码 Principal认证唯一标识 比如用户名、邮箱、身份证、手机号
        String username = (String)token.getPrincipal();
        //得到密码 凭证：密码
        String password = new String((char[])token.getCredentials());

        //自定义认证逻辑
        //假设从数据库中查询的用户名、密码
        String usernameDb = "bjsxt";
        String passwordDb = "111111";

        if(!usernameDb.equals(username)){
            throw new UnknownAccountException("该用户不存在");
        }
        if(!passwordDb.equals(password)){
            throw new IncorrectCredentialsException("请输入正确的密码");
        }
        return new SimpleAuthenticationInfo(username, password, getName());
    }

}

```
# 6. shiro与web、Spring的结合
## 6.1 基本步骤
1. 添加依赖
2. 修改`web.xml`,开启认证授权代理
3. 自定义`Realm`
4. 添加`applicationContext-shiro.xml`,配置密码凭证器、自定义的`realm`，打开`shiro`注解功能等

## 6.2 修改web.xml

```xml
  <filter>
    <!-- 此处与后续配置applicationContext-shiro.xml 过滤器名称一致 -->
    <filter-name>shiroFilter</filter-name>
    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
    <init-param>
        <!-- shiro过滤器生命周期，
        此值默认为false,表示生命周期由spring mvc
        当设置为true,表示生命周期交给servlet进行处理
        -->
        <param-name>targetFilterLifecycle</param-name>
        <param-value>true</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>shiroFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
```
## 6.3 配置applicationContext-shiro.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">
    <!-- 定义密码凭证匹配器 -->
    <bean id="credentialsMatcher" class="org.apache.shiro.authc.credential.HashedCredentialsMatcher">
        <!-- 指定散列算法名称 -->
        <property name="hashAlgorithmName" value="md5"/>
        <!-- 指定散列次数 -->
        <property name="hashIterations" value="2" />
    </bean>

    <!-- 定义realm -->
    <bean id="shiroRealm" class="com.bjsxt.shiro.ShiroRealm">
        <!-- 指定realm的密码凭证匹配器 -->
        <property name="credentialsMatcher" ref="credentialsMatcher" />
    </bean>

    <!-- 定义securityManager -->
    <bean id="securityManager" class="org.apache.shiro.web.mgt.DefaultWebSecurityManager">
        <!-- 指定realm -->
        <property name="realm" ref="shiroRealm"/>
    </bean>

    <!-- 此处的id 必须与web中filter-name一致 -->
    <bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">
        <!-- 指定securityManager -->
        <property name="securityManager" ref="securityManager" />
        <!-- loginUrl 需要登录时 跳转的url 可理解为默认访问的地址 非必须填 -->
        <property name="loginUrl" value="/login"/>
        <!-- successUrl 认证授权成功页面  -->
        <property name="successUrl" value="/index"/>
        <!--unauthorizedUrl 授权失败 跳转的页面 -->
        <property name="unauthorizedUrl" value="/error"/>

        <!-- filter chain -->
        <property name="filterChainDefinitions">
            <!-- 注意顺序
                anon:无需授权和认证即可访问 相当于是游客
                authc:必须经过授权之后，才能访问
            -->
            <value>
                /login=anon
                /index=authc
            </value>
        </property>
    </bean>
    <!-- 配置生命周期 -->
    <bean id="lifecycleBeanPostProcessor" class="org.apache.shiro.spring.LifecycleBeanPostProcessor"/>
    <!-- 开启注解 -->
    <bean class="org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator" depends-on="lifecycleBeanPostProcessor">
        <property name="proxyTargetClass" value="true" />
    </bean>
    <!-- 配置advisor -->
    <bean class="org.apache.shiro.spring.security.interceptor.AuthorizationAttributeSourceAdvisor ">
        <property name="securityManager" ref="securityManager"></property>
    </bean>
</beans>

```

## 6.4 自定义realm

```java
package com.bjsxt.shiro;

import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.authc.UnknownAccountException;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.authz.SimpleAuthorizationInfo;
import org.apache.shiro.crypto.hash.Md5Hash;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;
import org.apache.shiro.util.ByteSource;

/**
 * 继承AuthorizingRealm
 * @author zhiduo
 *
 */
public class ShiroRealm extends AuthorizingRealm {

    /**
     * 授权
     */
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        // TODO Auto-generated method stub
//      SimpleAuthorizationInfo info = new SimpleAuthorizationInfo();
        return null;
    }

    /**
     * 认证
     */
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        //获取用户名
        String userName = (String)token.getPrincipal();

        //模拟查询数据库 begin
        String usernameDb = "bjsxt";
        String passwordDb= "4773c912737e2a5599b466a658ab81ce";
        String saltDb = "1234";
        //模拟查询数据库 end

        if(!userName.equals(usernameDb)){
            throw new UnknownAccountException("该用户不存在");
        }
        //验证密码，由密码凭证匹配器自动完成
        return new SimpleAuthenticationInfo(userName,passwordDb,ByteSource.Util.bytes(saltDb.getBytes()),super.getName());
    }
}

```

## 6.5 UserController

```java
package com.bjsxt.controller;

import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.IncorrectCredentialsException;
import org.apache.shiro.authc.UnknownAccountException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.subject.Subject;
import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

@Controller
@RequestMapping("/user")
public class UserController {

    @RequestMapping(value = "/login", method = RequestMethod.POST)
    public String login(String username, String password,ModelMap modelMap) {
        //shiro管理登录 begin
        //得到subject
        try {
            Subject subject = SecurityUtils.getSubject();
            AuthenticationToken token = new UsernamePasswordToken(username, password);
            subject.login(token);
        } catch (AuthenticationException e) {
            String message="";
            if(e instanceof UnknownAccountException){
                message = "该账号不存在";
            }else if(e instanceof IncorrectCredentialsException){
                message = "密码不正确";
            }
            //生产环境中不建议提醒太准确
            modelMap.addAttribute("message", message);
            return "error";
        }
        //shiro管理登录 end
        return "redirect:/index";
    }
}


```
# 7. shiro缓存
## 7.1 基本步骤
1. 添加jar包、添加shiro-ehcache、ehcache-core
2. 添加ehcache的配置
3. 修改applicationContext-shiro.xml文件，开启realm缓存、session缓存。

## 7.2 添加`ehcache`配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache name="shirocache">

    <diskStore path="E:\shiroCache"/>

    <defaultCache
        maxElementsInMemory="1000"
        maxElementsOnDisk="10000000"
        eternal="false"
        overflowToDisk="true"
        diskPersistent="true"
        timeToIdleSeconds="120"
        timeToLiveSeconds="120"
        diskExpiryThreadIntervalSeconds="120"
        memoryStoreEvictionPolicy="LRU">
    </defaultCache>

</ehcache>
```
## 7.3 修改applicationContext_shiro.xml

```xml
<!-- 定义realm -->
    <bean id="shiroRealm" class="com.bjsxt.shiro.ShiroRealm">
        <!-- 指定realm的密码凭证匹配器 -->
        <property name="credentialsMatcher" ref="credentialsMatcher" />
        <!-- 开启缓存 CachingEnabled 默认为false -->
        <property name="cachingEnabled" value="true"/>
        <!-- 开启认证缓存 authenticationCachingEnabled 默认为false-->
        <property name="authenticationCachingEnabled" value="true"/>
        <!-- 开户授权缓存 authorizationCachingEnabled 默认为false -->
        <property name="authorizationCachingEnabled" value="true"/>
    </bean>

    <!-- 定义缓存管理器 -->
    <bean id="cacheManager" class="org.apache.shiro.cache.ehcache.EhCacheManager">
        <property name="cacheManagerConfigFile" value="classpath:shiro-ehcache.xml"/>
    </bean>

    <!-- 定义sessionManager 会自动的开启session级别的缓存 -->
    <bean id="sessionManager" class="org.apache.shiro.session.mgt.DefaultSessionManager"/>

    <!-- 定义securityManager -->
    <bean id="securityManager" class="org.apache.shiro.web.mgt.DefaultWebSecurityManager">
        <!-- 指定realm -->
        <property name="realm" ref="shiroRealm"/>
        <!-- 注入缓存管理器 -->
        <property name="cacheManager" ref="cacheManager"/>
        <!-- 注入session管理器 会自动的开启session级别的缓存 -->
        <property name="sessionManager" ref="sessionManager"/>
    </bean>

```