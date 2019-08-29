# Tomcat 配置連接池

## Tomcat 配置 JNDI 資源
JNDI(Java Naming and Directory Interface), **Java 命名和目錄接口**。JNDI 的作用就是: 在服務器上配置資源，然後通過統一的方式來獲取配置的資源。

## 配置 JNDI 資源要到 &lt;Context&gt; 元素中配置 &lt;Resource&gt; 子元素
- **name:** 指定資源的名稱，這個名稱可以隨便給，在獲取資源時需要這個名稱。
- **factory:** 用來創建資源的工廠，這個值基本上是固定的，不用修改。
- **type:** 資源的類型，我們要給出的類型當然是我們連接池的類型。
- **bar:** 表示資源的屬性，如果資源存在名為 bar 的屬性，那麼就配置 bar 的值。對於 DBCP 連接池而言，需要配置的不是 bar, 因為他沒有 bar 這個屬性，而是應該去配置 url, username 等屬性。

## Example
```java
//JDBC.xml
<Context>
	<!--
	name：指定资源的名称
	factory：资源由谁来负责创建
	type：资源的类型
	其他的东西都是资源的参数
	-->
	<Resource name="jdbc/dataSource"
			factory="org.apache.naming.factory.BeanFactory"
			type="com.mchange.v2.c3p0.ComboPooledDataSource"
			jdbcUrl="jdbc:mysql://localhost:3306/mydb1"
			driverClass="com.mysql.jdbc.Driver"
			user="root"
			password="tyler0131"
			acquireIncrement="5"
			initialPoolSize="10"
			/>
</Context>

//AServlet.java
package servlet;

import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;

import javax.naming.Context;
import javax.naming.InitialContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.sql.DataSource;

public class AServlet extends HttpServlet {

	public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		/*
		 * 1. 創建 JNDI 上下文對象
		 */
		try {
			Context  cxt = new InitialContext();
			//2. 查詢出入口
			Context envContext = (Context)cxt.lookup("java:comp/env");
			//3. 再進行二次查詢，找到我們的資源
			//使用的是名稱與<Resource>元素的 name 對應
			DataSource dataSource = (DataSource)envContext.lookup("jdbc/dataSource");
			
			Connection con = dataSource.getConnection();
			System.out.println(con);
			con.close();
		} catch(Exception e) {
			throw new RuntimeException(e);
		}
	}
}
```
