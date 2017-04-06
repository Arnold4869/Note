[TOC]
# 1. 基本语法
`${键名}`
## 1.1 获取用户请求信息

```jsp
${param.键名}
${paramValues.键名}
```

## 1.2 获取集合数据
### list集合
```jsp
${键名[角标]}
```
### Map集合
```jsp
${作用域中键名.集合键名}
```
# 2. EL表达式作用
- **获取四个作用域中的数据**
- **获取用户请求数据**

注意：不可以获得普通变量的值，只能获取作用域中的数据

# 3. EL获取作用域中数据的顺序
pageContext-->request-->session-->application

对于session,如果之前获取了一次，然后浏览器没关闭，服务器也没重启，由于session的特性，就算只有application，也会显示得到的是session的数据

# 4. 指定作用域获取数据
- `pageScope`获取`pageContext`作用域
- `requestScope`获取`request`作用域
- `sessionScope`获取`session`作用域
- `applicationScope`获取`application`作用域

# 5. EL表达式的运算
在EL表达式中，可以直接使用四则运算负直接进行运算，**不存在字符串的拼接**

# 6. empty 关键字