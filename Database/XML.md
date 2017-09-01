[TOC]

# 1. 先加入jdom.jar包
放在src即可

# 2. 创建XML文件
具体流程：
1. 创建节点对象
2. 写入节点数据
3. 生成节点关系
4. 生成文档对象
5. 将文档写出

```java
//创建节点对象
	Element root=new Element("person");
	Element name=new Element("name");
	Element nickname=new Element("nickname");
	Element sex=new Element("sex");
	Element fav=new Element("fav");
	Element wife=new Element("wife");
	//书写标签内容
	name.setText("张三");
	nickname.setText("张小三");
	sex.setText("男");
	fav.setText("唱歌");
	wife.setText("李四");
	//创建属性
	Attribute att=new Attribute("husband","张三");
	wife.setAttribute(att);

	//创建节点关系
	root.addContent(name);
	root.addContent(nickname);
	root.addContent(sex);
	root.addContent(fav);
	root.addContent(wife);
	//将XML文件写到指定的硬盘位置
	//生成文档
	Document doc=new Document(root);
	//将文档写出
	//Format f=Format.getCompactFormat();声明指定的输出格式
	Format f=Format.getPrettyFormat();
	XMLOutputter op=new XMLOutputter(f);
	op.output(doc, new FileOutputStream("d://1.xml"));
```
# 3. XML文件的读取和修改
具体流程：
1. 获取`document`对象
2. 获取**根节点**
3. 获取要修改的节点
4. 将修改的内容重新写出

```java
//创建XML的读取流对象,获取文档树对象
	SAXBuilder sb=new SAXBuilder();
	Document doc=sb.build(new FileInputStream(path));
	//获取根节点
	Element root=doc.getRootElement();
	//获取子节点
	Element name=root.getChild("name");
	Element wife=root.getChild("wife");
	Element son=root.getChild("son");
	Element s=son.getChild("son");
	Element sname=s.getChild("name");
	System.out.println(sname.getText());
	//修改节点内容
	name.setText("张大三");
	//修改节点属性
	wife.setAttribute("husband","张大三");
	//将修改后的内容重新写出
	Format f=Format.getPrettyFormat();
	XMLOutputter op=new XMLOutputter(f);
	op.output(doc, new FileOutputStream("d://1.xml"));
```