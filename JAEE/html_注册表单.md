[TOC]
### 作用
主要是用来收集表单域信息并提交给指定的处理程序
#### action:指定数据提交的程序，程序路径；
##### method:指定数据提交的方式（get/post）
get:
将数据以？隔开的形式拼接在URL后面进行提交。它是以键值对的进行提交，不通的键值对使用&符号隔开。数据提交依赖于表单域标签的name属性是否存在

缺陷：**数据不安全，不适合大流量的数据提交**

post:
隐藏的数据提交
安全，适合数据量大的，效率低

```html
<html>
	<head>
		<title >注册页面</title>
		<meta charset="UTF-8"/>
	</head>
	<body >
		<h3 align="center">注册</h3>
		<hr />
		<form action="#" method="get" >
			<table width="300px" align="center" border="0px" cellpadding="8px">
				<tr height="30px" >
					<td>用户名：</td>
					<td><input type="text" name="uname" id="uname" value="请输入用户名" /></td>
				</tr>
				<br />
				<br />
				<tr height="30px">
					<td width="150px" >密码：</td>
					<td width="150px" ><input type="password" name="pwd" id="pwd" value="" /></td>
				</tr>
				<tr height="30px">
					<td >手机号：</td>
					<td><input type="text" name="phone" id="phone" value="请输入手机号" /></td>
				</tr>
				<tr height="30px">
					<td>邮箱：</td>
					<td><input type="text" name="mail" id="mail" value="请输入邮箱" /></td>
				</tr>
				<tr height="30px" >
					<td>请选择性别</td>
					<td>
						男：
						<input type="radio" name="sex" id="sex" value="男" checked="checked" />
						女：
						<input type="radio" name="sex" id="sex" value="女" />
					</td>
				</tr>
				<tr height="30px">
					<td>籍贯</td>
					<td >
						<select name="address">
							<option value="0">-请选择-</option>
							<option value="1">--东--</option>
							<option value="2">--西--</option>
							<option value="3">--南--</option>
							<option value="4">--北--</option>
						</select>
					</td>
				</tr>
				<tr>
					<td>爱好：</td>
					<td>
					<input type="checkbox" name="fav" id="fac" value="吃" />吃
					<input type="checkbox" name="fav" id="fav" value="喝" />喝
					<input type="checkbox" name="fav" id="fav" value="睡" />睡
					</td>
				</tr>
				<tr height="30px">
					<td>自我介绍：</td>
				</tr>
				<tr height="30px">
					<td colspan="2">
						<textarea name="introduce" rows="8" cols="40"></textarea>
					</td>
				</tr>
				<tr>
					<td colspan="2" align="center">
						是否同意协议
						<input type="checkbox" name="" id="" value="是" />是
					</td>
				</tr>
				<tr>
					<td colspan="2" align="center">
						<input type="submit" name="" id="" value="注册" />
					</td>
				</tr>
			</table>
		</form>
	</body>
</html>
```

