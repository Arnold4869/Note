[TOC]

# 1. js的声明和引入
1. 在head标签中使用script标签声明js代码域
2. 在head标签中使用script标签引入外部声明好的js文件

```html
<!DOCTYPE html>
<html>
<head>
	<title>js引入</title>
	<script type="text/javascript">
		// js代码域
	</script>
	<script src="js文件的相对路径" type="text/javascript" charset="utf-8"></script>
	<!-- 不能二合一 -->
</head>
<body>

</body>
</html>
```

# 2. js的变量
## 2.1 变量的声明
在js中变量的声明只有关键字var，声明的变量可以接收任意类型的数据

### js为什么是弱类型语言
js运行在客户端，浏览器上，使用弱类型可以节省客户端在存储上的效率提升

## 2.2 变量的使用
- 命名和java一样，见名知意
- 变量名**严格区分大小写**
- 可以省略结束符`;`
- 可以有同名变量，但新值会覆盖旧值

## 2.3 数据的类型
js中变量没有类型，但数据是有类型的，常见的有：
- number:数字类型
- string:字符串类型
- boolean:布尔类型
- object:对象类型

查看某个变量存储的数据类型，使用关键字`typeof `

## 2.4 特殊的数据值
- undefined:在变量未赋值的情况下，默认是**undefined**
- NaN:在使用number()强转时，如果传入的不是一个数据，返回NaN（Not a Number）,并且NaN是**number**类型的
- null:空，是object类型的

# 3. 变量运算
## 3.1 基本运算
- 数值运算：+,-,*,/,%,++,--
- 链接符运算：+
- 逻辑运算：==,!=,>,<,>=,<=,&&,||,&,|,!

## 3.2 特殊运算
### 3.2.1 等值符：==
先判断**类型**，如果**类型**不一致，使用**number()**强转后再比较内容，内容一致则**true**,不一致**false**
如果类型一致，直接比较内容。内容一致**true**,内容不一致**false**

### 3.2.2 等同符：===
先判断**类型**，**类型**不一致直接**false**,类型一致再比较内容，内容一致则**true**,内容不一致**false**
# 4. 逻辑控制语句
和java一致，只不过在声明变量时，记得用var声明

# 5. 函数的声明和使用
## 5.1 函数的声明

声明一：
`function 函数名（形参1，形参2...）{执行体}`
声明二：
`var 变量名 = new Function("形参1"，"形参2"..."执行体");`
从这种方式可以看出，在js中函数也是对象
声明三：
`var 变量名 = function(形参1，形参2...){执行体}`

## 5.2 函数的参数
js中实参的传递个数可以和形参**个数不匹配**，这样**不会报错**，但是会根据**顺序**赋值

局部变量；在函数内部和形参声明的变量，在函数内有效
全局变量：在全局区声明的变量(即声明的js代码区)

## 5.3 函数的返回值
在js中如果有返回值，直接返回；没有返回值，则默认返回undefined

## 5.4 函数的执行符
函数的执行符就是`()`

## 5.5 函数作为参数传递
因为函数在js中也是对象，而对象是可以作为实参传递的，所以在js中函数也可以作为参数传递

**注意：如果传递时，没有使用执行符`()`，则函数作为对象使用，如果使用了执行符，则这个函数对象参数要作为函数执行**

# 6. 数组
# 6.1 数组的声明

```javascript
// 数组的声明
// 1.声明空数组
var arr1 = new Array();

// 2.声明指定长度的数组
var arr2 = new Array(5);

// 3.声明带有指定初始元素的数组
var arr3 = new Array(1,2,3,4,5);

// 4.直接声明
var arr4 = [1,2,3,4,5,6];
```

## 6.2 数组的使用

1. js的数组在使用的时候可以**不用指定长度**，即数组的长度不是固定的；
2. js的数组可以储存任意类型的数据；
3. 利用数组的角标进行**赋值**和**取值**；
4. 数组在进行赋值时，即使角标不是连续的，也不会报错，会自动用空补齐；在取值时，如果使用一个没有内容的角标，打印undefined

## 6.3 数组的length属性
获取数组的长度：**数组名.length**;
操作数组的长度：**数组名.length = 新长度**;
	注意：如果长度减小，自动从后进行缩减；如果长度增加，则自动用空补齐

## 6.4 数组的遍历
方式一：使用for循环
方式二：使用for-in循环；**注意：这种方式每次得到的是角标，不是元素**

```javascript
function testFor(){
	var arr = [1,"a", "b", 123];
	for (var i = 0; i < arr.length; i++) {
		alert(arr[i]);
	}
}

function testfor2(){
	var arr = [1,"a", "b", 123];
	for(var i in arr){
		alert(arr[i]);
	}
}

```
## 6.5 数组的常用方法
### concat方法
**使用concat方法链接数组**

```javascript
var a = [1,2,3,4];
var b = ["雾霾", "冬至", "饺子"];
var c = "javascript";
//使用concat方法链接数组
var d = a.concat(b,c);
```

### pop方法
**使用pop方法移除数组的最后一个元素**

```javascript
var a = [1,2,3,4];
alert("移除的元素是：" + a.pop());
alert("移除后的数组是：" + a);
```

### push方法
**在数组后边添加一个元素**

```javascript
var a =[1,2,3,4];
alert("使用push方法添加后的数组长度：" + a.push("饺子"));
alert("新数组的内容：" + a);
```

### shift方法
**在数组首位删除一个元素**

```javascript
var a = [1,2,3,4];
alert("使用shift方法移除的第一个元素是：" + a.shift());
alert("移除第一个元素后的数组是：" + a);
```

### unShift方法
**在数组首位添加元素**

```javascript
var a = [1,2,3,4];
alert("使用unShift方法在数组首位添加元素" + a.unShift("冬至"));
alert("添加后的数组内容为：" + a);
```
# 7. 常用对象和方法
## 7.1 String
1. substr(start,length)：从指定位置开始截取指定长度的字符串
2. substring(start,end)：从指定的开始位置和结束位置截取指定的字符串，左闭右开
3. split():按照指定的符号截取字符串

## 7.2 Date
1. getYear():获取的是从1900年距今的年数
2. getFullYear():获取的是当前年份
3. getMonth():获取的是月份，但是需要+1
4. getDay():获取的是星期数，星期日是0
5. getDate():获取当前日。比如21号
6. getHours():获取当前小时
7. getMinutes():获取当前小时
8. getSeconds():获取秒数

## 7.3 Math
使用Math的方法，当作java中的静态方法，直接Math.方法()就可以使用

- ceil(number):向上取整
- floor(number):向下取整
- random():0~1中的随机数字，左闭右开
- round():四舍五入

## 7.4 Global
注意：使用的时候不用new,也不用使用类名，直接使用方法即可

- eval():将字符串转换为可以执行的js代码
- parSeInt():截取字符串开头的整数
- parseFloat():截取字符串开头的小数
- isNaN():查看是否是NaN

# 8. 自定义类和对象

## 8.1 自定义类

```javascript
function 类名(形参1,形参2...){
	this.形参1 = 形参1;
	this.形参2 = 形参2;
	this.形参名 = 值;
	...
	this.函数名 = function(形参1,形参2...){
		执行体
	}
}
```
特点：
1. js的类在传参的时候可以不传完，也可以不传
2. js的类创建的对象是可扩充的对象，也就是除了类本身的拥有的属性和方法以外，对象还可以自定义属性和方法。
3. 可以使用prototype关键字变相的实现继承：
`类名1.prototype = new 类名2();`
这样实现后，类1的对象就可以调用类2中的属性和方法，看起来特别像类1继承了类2，但其实是通过类2的实例化对象来实现的调用。

## 8.2 自定义对象
创建自定义对象：

```javascript
var 对象名 = new Object();
var 对象名 = {};
对象名.属性1 = 值1;
对象名.属性2 = 值2;
对象名.属性3 = 值3;
对象名.方法名 = function(){
	执行体
}
```

# 9. 事件机制
满足一定条件会触发的某类事件的发生

```html
<!DOCTYPE html>
<html>
<head>
	<title></title>
	<meta charset="utf-8">
	<script type="text/javascript">
		function testOnclick(){
			alert("测试一下单击");
		}
	</script>
</head>
<body>
	<input type="button" value="测试" onclick="testOnclick();" >
</body>
</html>
```
## 9.1 常见事件
- onclick:单击事件
- ondblclick:双击事件
- onmouseout:鼠标移出事件
- onmouseover:鼠标悬停事件
- onkeydown:键盘下压事件
- onkeyup:键盘弹起事件
- onblur:失去焦点事件
- onfocus:获取焦点事件

## 9.2 超链接调用函数
href和使用js事件调用的区别
1. 使用**href**属性进行调用
`<a href = "javascript:函数名();"> 文字 </a>`
2. 使用js事件调用：不但会执行js函数，并且在函数执行完毕后会进行资源的跳转
`<a href = "资源URL" onclick="函数名();" target="_blank">随便说些啥</a>`

```html
<!DOCTYPE html>
<html>
<head>
	<title>js事件的调用</title>
	<meta charset="utf-8">
	<script type="text/javascript">
		function test(){
			alert("天气不错");
			return true;
		}
	</script>
</head>
<body>
	<!-- 使用超链接调用函数 -->
	<a href="http://www.baidu.com" target="_blank">百度一下</a>
	<!-- 使用js函数 -->
	<a href="javascript:test()">心情一下</a>
</body>
</html>
```

## 9.3 事件的阻断机制
在使用事件时添加返回值。如果返回**false**，则标签原有的功能会被阻断，返回**true**会继续执行

```html
<!DOCTYPE html>
<html>
<head>
	<title>js事件的调用</title>
	<meta charset="utf-8">
	<script type="text/javascript">
		function test(){
			alert("天气不错");
			return true;//这里如果是false，则不会执行跳转到百度
		}
	</script>
</head>
<body>
	<a href="http://www.baidu.com" onclick="return test();" target="_blank">测试啦</a>
</body>
</html>
```
## 9.4 事件对象
在触发DOM上的某个事件时，会产生一个事件对象event。这个对象中包含着所有与事件有关的信息。包括导致事件的元素，事件的类型以及其他与特定事件相关的信息。

通过下列代码得到鼠标的坐标

```javascript
function test1(){
	var eve = eve||window.eve;
	var x = eve.pageX||eve.x;
	var y = eve.pageY||eve.y;
	alert(x +":"+y);
}
```
通过以下代码得到键盘值（不是ASCII码）

```javascript
function test2(){
	var eve = eve||window.event;
	var kcode = eve.keycode;
	alert(kcode);
}
```
# 10. BOM对象模型
即浏览器对象模型（Brower Object Model）,是一个开发规范，规定了一些重要功能的统一性。
对它的学习就是在学习window对象的使用

## 10.1 框体方法
- alert();
- confirm():确认提示语;
- prompt():提示语、示例，填写了内容则返回内容，没有则返回空;

## 10.2 定时执行&间隔执行&关闭线程&关闭超时
**定时执行**

```html
<!DOCTYPE html>
<html>
<head>
	<title>定时执行&间隔执行</title>
	<meta charset="utf-8">
	<script type="text/javascript">
		//定义变量记录线程id
		var id;
		var num;
		//定时执行
		function testTimeOut(){
			num=window.setTimeout(function(){
				alert("定时执行");
			},2000);
		}
		//间隔执行
		function testInterval(){
			id=window.setInterval(function(){
				alert("间隔执行");
			},3000);
		}
		//使用clearInterval关闭线程
		function testClearInterval(){
			window.clearInterval(id);
		}
		function testClearTimeout(){
			window.clearTimeout(num);
		}
	</script>
</head>
<body>
	<input type="button" value="测试定时执行：setTimeout" onclick="testTimeOut();"/>
	<input type="button" value="测试间隔执行：setInterval" onclick="testInterval();"/>
	<hr />
	<input type="button" value="测试关闭setInterval:clearInterval" onclick="testClearInterval();"/>
	<input type="button" value="测试关闭setTimeout:clearTimeout" onclick="testClearTimeout();"/>
</body>
</html>
```

## 10.3 window对象学习
### open
格式如下
open("资源路径","子页面名","子窗口配置");
	打开子窗口
	资源路径：
		本地资源：相对路径
		网路资源：URL
	子窗口的配置：
		"height=400, width=600, top=200px,left=300px, toolbar=yes, menubar=yes,
		scrollbars=no, resizable=no,location=no, status=no"

```javascript
//打开网络资源
function testOpen(){
	window.open("http://www.baidu.com","new","height=400, width=600, top=200px,left=300px, toolbar=yes, menubar=yes, scrollbars=no, resizable=no,location=no, status=no")
}
//打开本地资源
function testOpen2(){
	window.open("son.html","son","height=400, width=600, top=200px,left=400px, toolbar=yes, menubar=yes, scrollbars=no, resizable=no,location=no, status=no");
}
```

### close
关闭子窗口
	注意，必须和open结合使用

```javascript
function closePage(){
	window.setTimeout(function(){
		window.close();
	},5000);
}
```

### opener
- 子类页面调用父类页面的函数
- 在子页面调用，在父页面执行
- 主要是用来做子父页面的资源统一

```javascript
function sonOpener(){
	window.opener.testOpener();//调用父页面的testOpener()函数
}
```

## 10.4 window属性
### location地址栏属性
- href:获取和操作地址栏信息
- reload:刷新当前页面
- port:端口号
- pathname:可设置或返回某个区域中URL的路径名
- hostname:主机IP

```javascript
function testLocation(){
	alert(window.location.href);
	window.location.href="http://www.baidu.com";
}
function test reload(){
	window.location.reload();
}
```
### screen

```javascript
function testScreen(){
	alert(window.screen.width+":"+window.screen.height);//得到分辨率
}
```
### history
- forward：页面前进
- back:页面后退
- go:跳转

```javascript
//页面前进
function testHistory1(){
	window.history.forward();
}
//页面后退
function testHistory2(){
	window.histroy.back();
}
//使用go
function testHistory3(){
	window.history.go(0);//跳转到当前页面，相当于当前页面刷新一下
}
```
### navigator
可以得到一些关于浏览器的信息

```javascript
function testNavigator(){
	alert(window.navigator.userAgent);
}
```
# 11. 闭包