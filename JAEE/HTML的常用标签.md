[TOC]

# 1.img标签
- src:引入的图片资源路径，相对路径
- width:设置图片的宽度
- height:设置图片的高度
- title:设置鼠标放在图片上的提示语
- alt:设置图片资源加载失败史的提示语

```html
<!DOCTYPE html>
<html>
<head>
	<title>img标签学习</title>
</head>
<body>
    <!--使用相对路径查找图片资源-->
	<img src="img/1.jpg">
	<img src="2.jpg">
	<!--使用网络资源  -->
	<img src="https://www.baidu.com/img/bd_logo1.png">
	<!-- 使用属性设置照片 -->
	<img src="4.jpg" width="600px" height="50px" title="这是一张风景图片" alt="图片加载失败时的提示语">
</body>
</html>
```

# 2 超链接标签

超链接标签

## 2.1 href：跳转的路径，可以是网络资源也可以是本地资源
## 2.2 target:设置资源打开的位置
- `_blank`表示在空白页打开
- `_self`当前页面打开（这是默认的方式
- `_top`在顶层页面打开资源
- `_parent`在父页面打开资源

## 2.3 锚点：
在当前页面跳转
在需要跳转的位置添加锚点：
`<a name = "名字" > </a>`
在使用a标签进行跳转
`<a href = "#姓名"> 访问方式</a>`

```html
<!DOCTYPE html>
<html>
<head>
	<title>超链接标签学习</title>
</head>
<body>
	<!--超链接 -->
	<a href="https://www.baidu.com" target="_blank">百度一下</a>
	<!-- 锚点 -->
	<a href="锚点.html">锚点学习啦</a>
</body>
</html>
```

# 3 表格
- table:声明一个表格
- tr:声明行
- td:声明列	普通单元格
- th:声明列	标题单元格（标题单元格内的文字自动加粗加黑）

## 3.1 表格的属性
- border:边框
- width:表格的宽
- height:表格的高度

注意事项
1. 第一行单元格的宽可以设置每列的宽度
2. 每行的高度就是此行单元格的高度

## 3.2 表格的合并
- 列合并 colspan,值为合并的单元格个数，将其它要合并的单元格删除
- 行合并 rowspan，值为合并的单元格个数，也需要将其它要合并的单元格删除

# 4 框架
## 4.1 iframe
在一个页面中可以再次嵌入一个页面，可以一个页面打开多个网页，如双搜的实现

```html
<!DOCTYPE html>
<html>
<head>
	<title>iframe标签</title>
</head>
<body>
	<a href="www.taobao.com" target="_ifm" > 淘宝网</a>
	<br/>
	<iframe src="www.baidu.com" width="100%" height="400px" name="_ifm"></iframe>
</body>
</html>
```
## 4.2 frameset
可以把一个页面分隔
用法：
1. 先删除body标签
2. 划分页面
3. 使用fram子标签进行页面占位
4. frameset中还可以继续使用frameset

**注意：在做退出功能时，使用target属性的_parent或者_top**

```html
<!DOCTYPE html>
<html>
<head>
	<title></title>
</head>
<frameset rows="10%,*,10%">
	<frame src="mail/top.html">
	<frameset cols="10%,*">
		<frame src = "mail/left.html"></frame>
		<frame src="mail/right.html" name="right"></frame>
	</frameset>
	</frame src="mail/botton.html">
</frameset>
</html>
```
