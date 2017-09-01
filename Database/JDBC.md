[TOC]
# JDBC

## 1.什么是JDBC
Java Data Base Connectivity ：java数据库连接，它是用来执行sql语句的java API，为多种关系型数据库提供统一访问，它由一组用java语言编写的类和接口组成

## 2.使用JDBC

### 2.1使用JDBC插入数据

1. 加载驱动
2. 创建数据库连接
3. 创建Statement并发送命令
4. 处理ResultSet结果
5. 关闭数据库资源

```java
package com.bjsxt.testjdbc;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class Test01 {
	public static void main(String[] args) throws ClassNotFoundException, SQLException {
		//加载JDBC驱动
		Class.forName("oracle.jdbc.driver.OracleDriver");
		//获取链接
		Connection conn = DriverManager.getConnection("jdbc:oracle:thin:@localhost:1521/orcl", "scott", "tiger");
		//发送并执行sql命令
		String sql = "insert into dept values('50','教学部','Beijing')";
		Statement stmt = conn.createStatement();
		//获取结果
		int i = stmt.executeUpdate(sql);
		//处理结果
		if(i > 0){
			System.out.println("插入成功");
		}else{
			System.out.println("插入失败");
		}
		//关闭资源,有顺序
		stmt.close();
		conn.close();
	}
}

```
