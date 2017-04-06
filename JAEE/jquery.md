[TOC]
## 1. jQuery的引入
1. 只有一个全局代码区，声明变量的时候出现同名的概率比较高，也就是容易造成覆盖
2. 使用命名空间,将代码挂载到某个对象下，但是仍然存在对象被覆盖的风险
3. 工厂模式封装：将对象封存起来，让代码更加的具备完整性，但是依然存在被覆盖的风险
4. 不要函数名字，虽然解决了被覆盖的风险，但是无法进行调用了
5. 使用自己调用自己的方式可以实现函数的调用，但是只能调用一次，在页面加载的时候
6. 闭包原理：使用更大作用域变量或者大家同一认可的变量将执行一次就会消失的内容记录下来，实现重复调用

## 2. 选择器
1. **ID选择器：`$("#id名")`**
2. **标签选择器：`$("标签名")`**
3. 层级选择器
4. 角标选择器
5. 属性选择器
6. 表单选择器
7. 表单属性选择器

被**引号**包裹的和CSS选择器类似

## 3. 操作元素的属性
步骤如下
1. 获取元素对象
2. 使用attr()函数获取属性内容
`对象名.attr("属性名")`
3. 修改属性内容
`对象名.attr("属性名"，"属性值")`

**注意：`attr()`函数获取的是数据的默认值，value的值不能试试获取，如果想实时获取，需要使用`val()`函数**

## 4. 操作元素的内容
1. 获取元素对象
2. 操作元素的内容

`对象名.html()`类似于innerHTML
`对象名.text()`则只会处理之间的文本内容，不对标签做修改

需要添加数据使用类似于如下的格式：
`对象名.html(对象名.html()+"新值")`

## 5. 操作元素的样式
可以直接使用css操作样式

```js
	function testCss(){
		//获取元素对象
		var showdiv=$("#showdiv");
		//获取元素的样式
		alert(showdiv.css("color"));
		//修改元素的样式
		showdiv.css("color","blue");
		//添加元素的样式
		/*showdiv.css("width","200px");
		showdiv.css("height","200px");*/
		showdiv.css({width:"200px",height:"200px"});
	}
	//使用addClass操作样式
	function testAddClass(){
		//获取元素对象
		var div01=$("#div01");
		//使用addClass增加样式
			//div01.addClass("common");
		//使用removeClass删除类样式
		div01.removeClass("common");
	}
```
## 6. 操作元素的文档结构
常用的如下：
- append
- preappend
- appendTo
- prependTo
- after
- before
- empty

## 7. 操作事件
### 7.1 bind:添加永久事件
`对象名.bind("事件名",时间函数)`

```js
function testBind(){
	var btn = $("#btn");
	btn.bind("click",function(){
		alert("我是btn");
		})
}
```
**可以使用unbind解绑**
`btn.unbind("click")`

### 7.2 one：添加一次性事件
`对象名.one("事件名",事件函数)`

### 7.3 使用事件函数添加
`对象名.事件名(事件函数)`

```js
function testThing(){
	var btn = $("#btn");
	btn.click(function(){
		alert("我是事件函数");
		})
}
```

### 7.4 页面加载事件

**`$(document).ready(页面加载执行的函数）`**

## 8. 动画效果
`toggle`,`slidedown`,`fadein`,`fadeout`

```html
<html>
	<head>
		<title>jquery的动画效果</title>
		<meta charset="UTF-8"/>
		<script src="js/jquery-1.9.1.js" type="text/javascript" charset="utf-8"></script>
		<style type="text/css">
			#showdiv{
				border: solid 1px;
				height: 300px;
				background-color: aquamarine;
			}
			#div01{
				border: solid 1px;
				height: 300px;
				background-color:blueviolet;
			}
		</style>
		<script type="text/javascript">
			function testShow(){
				//获取元素对象
				var showdiv=$("#showdiv");
				//使用show函数
					//showdiv.show(2000);
				//使用hide()
					//showdiv.hide(2000);
					$("div").toggle(3000);
					$("div").toggle(3000);
					$("#showdiv").slideDown(3000);
					$("#showdiv").slideUp(3000);
				$("#showdiv").fadeIn(3000);
				$("#div01").fadeOut(3000);
			}
		</script>
	</head>
	<body onload="testShow()">
		<div id="showdiv" style="display: none;">

		</div>
		<div id="div01">

		</div>
	</body>
</html>
```