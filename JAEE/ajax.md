[TOC]
# 1.基本流程

```js
function testAjax(){
	//创建Ajax引擎
	var ajax;
	if(window.XMLHttpRequest){
		ajax = new XMLHttpRequest();
	}else if (window.ActiveXObject){
		ajax = new ActiveXObject("Msxml2.XMLHTTP");
	}
	//覆写onreadystatuschange函数用来处理请求
	ajax.onreadystatuschange = function(){
		/*
			0:请求未初始化
			1：服务器连接已建立
			2：请求已提交
			3：请求处理中
			4：请求已完成，且响应已就绪
		*/
		if(ajax.readyState == 4){
			//200表示正常访问
			if(ajax.status == 200){
				//普通文本方式返回的数据，
				var result = ajax.responseText;
				var showdiv = document.getElementById("showdiv");
				showdiv.innerHTML = result;
			}else if (ajax.status == 404){

			}else if (ajax.status == 500){

			}
		}
	}
	//使用ajax引擎发送请求
	ajax.open("get","ajax.action");
	ajax.send(null);
}
```
## 2.响应的数据格式
### 2.1 普通文本

### 2.2 json
可以使用`eval`直接将符合格式的json文本转换为对应的对象

```js
var result = ajax.responseText;
eval("var data = " + result);
```
### 2.3 使用XML
前端

```js
var doc = responseXML;
alert(doc.getElementByTagName("name").length);
alert(doc.getElementByTagName("name")[1].firstChild.data);
```
服务器端

```js
resp.setContentType("text/xml;charset=utf-8");
resp.setCharacterEncoding("utf-8");

resp.getWriter().write("<person><name>李四</name><age>18</age></person>");
```
# 3. js下的ajax

```js
function testAjaxS1() {
	$.post("user/user!testAjaxS1.action", {uname:"zhangsan",realname:"张三"}, function(data) {
		alert(data);
	});
}
function testAjaxS2() {
	$.get("user/user!testAjaxS2.action", {uname:"lisi",realname:"李四"}, function(data) {
		alert(data);
	});
}
```