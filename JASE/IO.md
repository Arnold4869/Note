[TOC]
# 分类

从输入和输出可以分为**输入流**和**输出流**
从读取和写入的类型可以分为**字节流**和**字符流**

# 字节流 InputStream 与 OutputStream

分为输入流和输出流，一般是配套使用。

## 过滤器类
### DataInputStream 与 DataOuputStream

搭配起来可以将基本数据类型和 String 转移到指定的地方，具体的地方，由 InputStream 和 OutputStream 的那些子类决定。

## ObjectInputStream 与 ObjectOutputStream

与 DataInputStream 和 DataOuputStream 相对，用来处理**引用数据类型**。

```java
//1.放入String类型数据：
public class Test002 {
	public static void main(String[] args) throws FileNotFoundException, IOException {
		ObjectOutputStream oos=new ObjectOutputStream(new FileOutputStream(new File("d:/bjsxt/t.txt")));
		oos.writeObject("java");
		oos.close();
	}
}
//2.现在要写入Person(需要实现序列化)的一个对象：
public class Test002 {
	public static void main(String[] args) throws FileNotFoundException, IOException {
		ObjectOutputStream oos=new ObjectOutputStream(new FileOutputStream(new File("d:/bjsxt/t.txt")));
//		oos.writeObject("java");
		oos.writeObject(new Person("lili", 18));
		oos.close();
	}
}
```
### 其它过滤器类

修改 InputStream 和 OutputStream 的**内部行为方式**：是否**缓冲**，是否**保留读过的行**，是否**把一个单一字符推回输入流**等

常用的是 BufferInputStream 和 BufferedOutputStream
#### BufferInputStream 与 BufferedOutputStream

```java
public class Test001 {
	public static void main(String[] args) throws IOException {
		//1.源文件，目标文件
		File f1=new File("d:/bjsxt/test001.txt");
		File f2=new File("d:/bjsxt/demo.txt");
		//2.创建流：4个
		FileInputStream fis =new FileInputStream(f1);
		FileOutputStream fos=new FileOutputStream(f2);
		BufferedInputStream bis=new BufferedInputStream(fis);
		BufferedOutputStream bos=new BufferedOutputStream(fos);
		//3.读取
		byte[] b=new byte[8];
		int len=bis.read(b);
		while(len!=-1){
			bos.write(b,0,len);
			len=bis.read(b);
		}
		//4.关闭--先关高级流，再关闭低级流
		//(方式1：)bos.flush();---同样有缓存机制，需要自己将信息怼出去
		bis.close();
		bos.close();
		fis.close();
		fos.close();

	}
}
```

# 字符流 Reader 与 Writer

字符流只能处理简单的文本文件（ word 文档和 PDF 因为都可以包含图片，不能使用字符流处理）

**基本原理就是把底层的读写的单位从字节换成了字符（byte-->char）**


# 字节流与字符流区别
字节流可以处理所有二进制数据文件
字符流只能处理文本
## read() 与 readLine()
`read()` 与它的重载方法，基本所有输入流都有，并且都是判定在返回值为 **-1**时读到了末尾；
`readLine()`则只存在于 `BufferedReader` 中，并且是判定返回值为 `null`时读到了文件末尾。

# 使用场景
参见《Thinking in Java》P539-P545

```java
DataInputStream din = new DataInputStream(new BufferedInputStream(new FileInputStream("employee.dat")));
double s = din.readDouble();
```
