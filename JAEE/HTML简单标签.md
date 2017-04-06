[TOC]
# 1. head

## 1.1 title

## 1.2 meta基本信息
用来标识网页的基本信息

```html

<html>
<head>
	<title></title>
	<meta name = "keywords" content="Java培训,尚学堂">
	<meta name = "description" content="blablabla">
	<meta name = "author" content="Arnold">
	<meta http-equiv="content-type" content="text/html;charset = utf-8">
	<meta http-equiv = "refresh" content="3;url = www.baidu.com">
</head>
<body>

</body>
</html>
```

# 2 body

## 2.1 标题h1~h6

```html

```
## 2.2 段落 p
```html
<p align="center">
	align设置排版方式，现在是居中
</p>
```
## 2.3 分割线hr(单标记)


```html
<hr sizi = "5px" width="600px" color = "gray" align="lefy" />
```
## 2.4 空格 %nbsp
## 2.5 预格式化 pre

对于pre标签修饰的内容，原样输出

```html
<!DOCTYPE html>
<html>
<head>
	<title></title>
</head>
<body>
	<pre>
		锄禾
		锄禾日当午，
		汗滴禾下土。
		谁知盘中餐，
		粒粒皆辛苦。
	</pre>
</body>
</html>
```
## 2.6 段落

`<p> </p>`

## 2.7 列表

无序列表

```html
<!DOCTYPE html>
<html>
<head>
	<title></title>
</head>
<body>
	<ul>
		<li>河北</li>
		<li>云南</li>
		<li>黑龙江</li>
	</ul>
</body>
</html>
```

有序列表

```html
<!DOCTYPE html>
<html>
<head>
	<title></title>
</head>
<body>
	<ol type="I">
	<!-- I表示罗马数字计数，相应的1可以表示为阿拉伯数字技术 -->
		<li>北京</li>
		<li>上海</li>
		<li>广州</li>
	</ol>
</body>
</html>
```
其中，可以用type属性设置列表的类型。
## 2.8 字体标签

```html
<!DOCTYPE html>
<html>
<head>
	<title></title>
</head>
<body>
	<b>这是粗体</b>
	<i>这是斜体</i>
	<u>这是下划线</u>
	<del>这是删除线</del>
</body>
</html>
```
