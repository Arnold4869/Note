[TOC]
# 1. 作用
**用来在jsp页面上替换java代码，是一种使用标签学习来实现java功能的语言**

需要先在上方引入：

```jsp
<%@taglib prefix = "c" url=	"http://java.sun.com/jsp/jstl/core"%>
```

# 2. 标签库
## 2.1 核心标签库(重点)
### 2.1.1 输出标签：out
**value**:要输出的值用EL表达式去作用域中取出
**default**:当作用域中没有找到指定的数据，输出默认值到客户端

```jsp
<c:out value="${a}"></c:out>
<c:out value="${c}" default="没有找到符合要求的值"></c:out>
```
### 2.1.2 作用域中存储标签：set
- **var**:键名
- **value**:值
- **scope**:指定的作用域`pageScope,requestScope,sessionScope,applicationScope`

```jsp
<c:set var = "b" value = "111" scope = "requeset"></c:set>
<c:out value="${requestScope.b}></c:out>"
```

### 2.1.3 删除作用域中的数据：remove
- **var**:键名，在**不指定作用域**的情况下，将四个作用域中该键名的数据**全部删除**
- **scope**:删除指定的作用域

```jsp
<c:remove var = "b" scope="request"/>
```

### 2.1.4 逻辑判断

#### if

```jsp
<c:set var="a" value="52"></c:set>
	<c:if test="${a==1}">
			<b>最近雾霾大vvv了。。。</b>
	</c:if>
```

#### choose:相当于if else
格式：

```jsp
<c:choose>
	<c:when test=${逻辑表达式}>//逻辑表达式中的值，必须是存在于作用域中的
		执行语句
	<c:when>
	<c:when test=${逻辑表达式}>//逻辑表达式中的值，必须是存在于作用域中的
		执行语句
	<c:when>
	<c:otherwise>
		执行语句
	<c:otherwise>
	otherwise
</c:choose>
```

#### forEach循环
- **items**:${键名} 获取要循环的对象集合，而且不用指定循环范围，自动全部遍历
- **begin**:开始遍历的角标
- **end**:结束遍历的角标
- **step**:步长
- **var**:记录每次遍历的结果
- **varStatus**:记录每次遍历的状态

对于`vatStatus`:
1. **index**:每次遍历的角标
2. **count**:每次遍历的次数
3. **first**:是否是第一次遍历
4. **last**:是否是最后一次遍历

```jsp
<c:forEach begin="1" end="8" step="2" var="i" varStatus="s">
	<c:if test="${s.index==1}">
		222--${i}----${s.index}---${s.count}---${s.first}---${s.last}<br/>
	</c:if>
</c:forEach>
```

## 2.2 其它标签库(无需掌握)
- 函数标签库
- XML标签库
- SQL标签库
- 格式化标签库