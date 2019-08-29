# JDBC 增、刪、改、查

## Example
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

import org.junit.Test;


public class Demo2 {
	/*
	 * 連接數據庫: 得到 Connection。
	 * 對數據庫進行增刪改
	 */
	@Test
	public void fun1() throws ClassNotFoundException, SQLException {
		/*
		 * 1. 準備四大參數
		 * 2. 加載驅動類
		 * 3. 得到 Connection
		 */
		//準備四大參數
		String driverClassName = "com.mysql.jdbc.Driver";
		// jdbc 協議的格式
		String url = "jdbc:mysql://localhost:3306/mydb1";
		String username = "root";
		String password = "tyler0131";
		
		//加載驅動類
		Class.forName(driverClassName);
		//使用 DriverManager，以及剩下三個參數，得到 Connection
		Connection con = DriverManager.getConnection(url, username, password);
		
		/*
		 * 對數據庫進行增刪改
		 * 1. 通過 Connection 對象創建 Statement
		 * 	  - Statement 語句的發送器，他的功能就是向數據庫發送 SQL 語句
		 * 2. 調用他的 int executeUpdate(String sql)，他可以發送DML, DDL
		 */
		//通過 Connection 得到 Statement 對象
		Statement stmt = con.createStatement();
		//使用 Statement 發送 SQL 語句
//		String sql = "INSERT INTO stu VALUES(870156, 'Hello', 80, 'Male')";
//		String sql = "DELETE FROM stu WHERE name='Hello'";
//		String sql = "UPDATE stu SET age=50 WHERE name='Hello'";
//		int r = stmt.executeUpdate(sql);
//		System.out.println(r);
	}
	
	/**
	 * 執行查詢
	 * @throws ClassNotFoundException 
	 * @throws SQLException 
	 */
	@Test
	public void fun2() throws ClassNotFoundException, SQLException {
		/*
		 * 1. 得到 Connection
		 * 2. 得到 Statement, 發送 select 語句
		 * 3. 對查詢返回的"表格"進行解析
		 */
		/*
		 * 得到連接
		 * 1. 準備四大參數
		 */
		String driverClassName = "com.mysql.jdbc.Driver";
		String url = "jdbc:mysql://localhost:3306/mydb1";
		String username = "root";
		String password = "tyler0131";
		
		/*
		 * 加載驅動類
		 */
		Class.forName(driverClassName);
		/*
		 * 通過剩下3個參數調用DriverManager的getConnection(), 得到連接
		 */
		Connection con = DriverManager.getConnection(url, username, password);
		
		/*
		 * 得到 Statement
		 * 1. 得到 Statement 對象，Connection 的 createStatement() 方法
		 */
		Statement stmt = con.createStatement();
		/*
		 * 2. 調用 Statement 的 ResultSet rs = executeQuery(String querySql);
		 */
		ResultSet rs = stmt.executeQuery("SELECT * FROM stu");
		/*
		 * 解析 ResultSet
		 * 1. 把行光標移動到第一行
		 */
		while(rs.next()) { //把光標向下移動一行，並判斷下一行是否存在
			int stuno = rs.getInt(1); //通過列編號來獲取該列的值
			String stuname = rs.getString("Name");
			double age = rs.getDouble("Age");
			
			System.out.println(stuno + ", " + stuname + ", " + age);
		}
		/*
		 * 關閉資源
		 * 倒著關
		 */
		rs.close();
		stmt.close();
		con.close(); //Connection 對象必須要關!
	}
}
```
