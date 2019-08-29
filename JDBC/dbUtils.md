# dbUtils

## Example
```java
//Stu.java
package dbutils;

public class Stu {
	private int sid;
	private String sname;
	private int age;
	private String gender;
	public int getSid() {
		return sid;
	}
	public void setSid(int sid) {
		this.sid = sid;
	}
	public String getSname() {
		return sname;
	}
	public void setSname(String sname) {
		this.sname = sname;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	public String getGender() {
		return gender;
	}
	public void setGender(String gender) {
		this.gender = gender;
	}
	@Override
	public String toString() {
		return "Stu [sid=" + sid + ", sname=" + sname + ", age=" + age
				+ ", gender=" + gender + "]";
	}
	public Stu() {
		super();
		// TODO Auto-generated constructor stub
	}
	public Stu(int sid, String sname, int age, String gender) {
		super();
		this.sid = sid;
		this.sname = sname;
		this.age = age;
		this.gender = gender;
	}
}

//QR.java
package dbutils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

import javax.sql.DataSource;

public class QR<T> {
	private DataSource dataSource;
	
	public QR(DataSource dataSource) {
		this.dataSource = dataSource;
	}

	public QR() {
		super();
	}

	/**
	 * 做insert、update、delete
	 * @param sql
	 * @param params
	 * @return
	 */
	public int update(String sql, Object... params) {
		Connection con = null;
		PreparedStatement pstmt = null;
		try {
			con = dataSource.getConnection();//通过连接池得到连接对象

			pstmt = con.prepareStatement(sql);//使用sql创建pstmt
			initParams(pstmt, params);//设置参数
			
			return pstmt.executeUpdate();//执行
		} catch(Exception e) {
			throw new RuntimeException(e);
		} finally {
			try {
				if(pstmt != null) pstmt.close();
				if(con != null) con.close();
			} catch(SQLException e1) {}
		}
	}
	
	// 给参数赋值
	private void initParams(PreparedStatement pstmt, Object... params) 
			throws SQLException {
		for(int i = 0; i < params.length; i++) {
			pstmt.setObject(i+1, params[i]);
		}
	}
	
	public T query(String sql, RsHandler<T> rh, Object... params) {
		Connection con = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;
		try {
			con = dataSource.getConnection();//通过连接池得到连接对象

			pstmt = con.prepareStatement(sql);//使用sql创建pstmt
			initParams(pstmt, params);//设置参数
			
			rs = pstmt.executeQuery();//执行
			
			return rh.handle(rs);
		} catch(Exception e) {
			throw new RuntimeException(e);
		} finally {
			try {
				if(pstmt != null) pstmt.close();
				if(con != null) con.close();
			} catch(SQLException e1) {}
		}
	}
}

// 用来把结果集转换成需要的类型！
interface RsHandler<T> {
	public T handle(ResultSet rs) throws SQLException;
}


//Demo1.java
package dbutils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.List;

import org.junit.Test;

import demo8.JdbcUtils;

/**
 * commons-dbutils
 * * 简化jdbc的代码！
 * @author cxf
 *
 */
public class Demo1 {
	@Test
	public void addStu(Stu stu) {
		Connection con = null;
		PreparedStatement pstmt = null;
		try {
			con = JdbcUtils.getConnection();
			
			String sql = "insert into t_user values(?,?,?,?)";
			pstmt = con.prepareStatement(sql);
			pstmt.setInt(1, stu.getSid());
			pstmt.setString(2, stu.getSname());
			pstmt.setInt(3, stu.getAge());
			pstmt.setString(4, stu.getGender());
			
			pstmt.executeUpdate();
		} catch(Exception e) {
			//处理异常
		} finally {
			//关闭
		}
	}
	
	@Test
	public void updateStu(Stu stu) {
		Connection con = null;
		PreparedStatement pstmt = null;
		try {
			con = JdbcUtils.getConnection();
			
			String sql = "update t_stu set sname=?, age=?, gender=? where sid=?";
			pstmt = con.prepareStatement(sql);
			pstmt.setInt(4, stu.getSid());
			pstmt.setString(1, stu.getSname());
			pstmt.setInt(2, stu.getAge());
			pstmt.setString(3, stu.getGender());
			
			pstmt.executeUpdate();
		} catch(Exception e) {
			//处理异常
		} finally {
			//关闭
		}
	}
	
	@Test
	public void deleteStu(int sid) {
		Connection con = null;
		PreparedStatement pstmt = null;
		try {
			con = JdbcUtils.getConnection();
			
			String sql = "delete from t_stu where sid=?";
			pstmt = con.prepareStatement(sql);
			pstmt.setInt(1, sid);
			
			pstmt.executeUpdate();
		} catch(Exception e) {
			//处理异常
		} finally {
			//关闭
		}
	}
	
	public Stu load(int sid) {
		Connection con = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;
		try {
			con = JdbcUtils.getConnection();
			
			String sql = "select * from t_stu where sid=?";
			pstmt = con.prepareStatement(sql);
			pstmt.setInt(1, sid);
			
			rs = pstmt.executeQuery();
			if(!rs.next()) return null;
			
			/*
			 * 需要把rs转换成Stu对象
			 * rs --> javabean
			 */
			Stu stu = new Stu();
			stu.setSid(rs.getInt("sid"));
			stu.setSname(rs.getString("sname"));
			stu.setAge(rs.getInt("age"));
			stu.setGender(rs.getString("gender"));
			
			return stu;
		} catch(Exception e) {
			throw new RuntimeException(e);
		} finally {
			//关闭
		}
	}
}

//Demo2.java
package dbutils;

import java.sql.ResultSet;
import java.sql.SQLException;

import org.junit.Test;

import demo8.JdbcUtils;

public class Demo2 {
	@Test
	public void fun1() {
//		Stu s = new Stu(1001, "zhangSan", 99, "male");
//		addStu(s);
		
		Stu s = load(1001);
		System.out.println(s);
	}
	
	public void addStu(Stu stu) {
		QR qr = new QR(JdbcUtils.getDataSource());//创建对象时给出连接池
		String sql = "insert into t_stu values(?,?,?,?)";//给出sql模板
		// 给出参数
		Object[] params = {stu.getSid(), stu.getSname(), stu.getAge(), stu.getGender()};
		// 调用update执行增、删、改！
		qr.update(sql, params);
	}
	
	public Stu load(int sid) {
		QR qr = new QR(JdbcUtils.getDataSource());//创建对象时给出连接池
		String sql = "select * from t_stu where sid=?";//给出sql模板
		Object[] params = {sid};
		
		RsHandler<Stu> rh = new RsHandler<Stu>() {
			public Stu handle(ResultSet rs) throws SQLException {
				if(!rs.next()) return null;
				Stu stu = new Stu();
				stu.setSid(rs.getInt("sid"));
				stu.setSname(rs.getString("sname"));
				stu.setAge(rs.getInt("age"));
				stu.setGender(rs.getString("gender"));
				return stu;
			}
		};
		return (Stu) qr.query(sql, rh, params);
	}
}

//Demo3.java
package dbutils;

import java.sql.SQLException;
import java.util.List;
import java.util.Map;

import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanHandler;
import org.apache.commons.dbutils.handlers.BeanListHandler;
import org.apache.commons.dbutils.handlers.MapHandler;
import org.apache.commons.dbutils.handlers.MapListHandler;
import org.apache.commons.dbutils.handlers.ScalarHandler;
import org.junit.Test;

import demo8.JdbcUtils;

public class Demo3 {
	@Test
	public void fun1() throws SQLException {
		QueryRunner qr = new QueryRunner(JdbcUtils.getDataSource());
		String sql = "insert into t_stu values(?,?,?,?)";
		Object[] params = {1002, "liSi", 88, "female"};
		
		qr.update(sql, params);
	}
	
	@Test
	public void fun2() throws SQLException {
		// 创建QueryRunner，需要提供数据库连接池对象
		QueryRunner qr = new QueryRunner(JdbcUtils.getDataSource());
		// 给出sql模板
		String sql = "select * from t_stu where sid=?";
		// 给出参数
		Object[] params = {1001};
		
//		ResultSetHandler<Stu> rsh = new ResultSetHandler<Stu>() {
//
//			@Override
//			public Stu handle(ResultSet rs) throws SQLException {
//				// TODO Auto-generated method stub
//				return null;
//			}
//		};
		// 执行query()方法，需要给出结果集处理器，即ResultSetHandler的实现类对象
		// 我们给的是BeanHandler，它实现了ResultSetHandler
		// 它需要一个类型，然后它会把rs中的数据封装到指定类型的javabean对象中，然后返回javabean
		Stu stu = qr.query(sql, new BeanHandler<Stu>(Stu.class), params);
		System.out.println(stu);
	}
	
	/**
	 * BeanListHandler的应用，它是多行处理器
	 * 每行对象一个Stu对象！
	 * @throws Exception
	 */
	@Test
	public void fun3() throws Exception {
		QueryRunner qr = new QueryRunner(JdbcUtils.getDataSource());
		String sql = "select * from t_stu";
		List<Stu> stuList = qr.query(sql, new BeanListHandler<Stu>(Stu.class));
		
		System.out.println(stuList);
	}
	
	/**
	 * MapHandler的应用，它是单行处理器，把一行转换成一个Map对象
	 * @throws SQLException 
	 */
	@Test
	public void fun4() throws SQLException  {
		QueryRunner qr = new QueryRunner(JdbcUtils.getDataSource());
		String sql = "select * from t_stu where sid=?";
		Object[] params = {1001};
		Map map = qr.query(sql, new MapHandler(), params);
		
		System.out.println(map);
	}
	
	/**
	 * MapListHandler，它是多行处理器，把每行都转换成一个Map，即List<Map>
	 * @throws SQLException
	 */
	@Test
	public void fun5() throws SQLException  {
		QueryRunner qr = new QueryRunner(JdbcUtils.getDataSource());
		String sql = "select * from t_stu";
		List<Map<String,Object>> mapList = qr.query(sql, new MapListHandler());
		
		System.out.println(mapList);
	}
	
	/**
	 * ScalarHandler，它是单行单列时使用，最为合适！
	 * @throws SQLException
	 */
	@Test
	public void fun6() throws SQLException {
		QueryRunner qr = new QueryRunner(JdbcUtils.getDataSource());
		String sql = "select count(*) from t_stu";
		/*
		 * Integer、Long、BigInteger
		 */
		Number cnt = (Number)qr.query(sql, new ScalarHandler());
		
		long c = cnt.longValue();
		System.out.println(c);
	}
}
``` 

## common-dbutils.jar
**QueryRunner**

- **update 方法**
    - int update(String sql, Object[] params) -> 可執行增刪改語句。
    - int update(Connection con, String sql, Object...params) -> 需要調用者提供 Connection, 這說明本方法不再管理 Connection 了，**支持事務**。

- **query 方法**
    - T query(String sql, ResultSetHandler rsh, Object...params) -> 可執行查詢
        - 他會先得到 ResultSet, 然後調用 rsh 的 handle() 把 rs 轉換成需要的類型。
    - T query(Connection con, String sql, ResultSetHandler rsh, Object...params), **支持事務**。

- **ResultSetHandler 接口**
    - BeanHandler(單行) -> 構造器需要一個 Class 類型的參數，用來把結果轉換成指定類型的 javaBean 對象
    - BeanListHandler(多行) -> 構造器也是需要一個 Class 類型的參數，用來把一行結果集轉換成 javabean, 那麼多行轉換成 List 對象，一堆 javabean 
    - MapHandler(單行) -> 把一行結果集轉換成 Map 對象。
      一行紀錄:

        ```
        sid  sname  age  sex
        1001  jack   20   male
        ```
      一個 Map:
        ```
        {sid:1001, sname:jack, age:20, sex:male}
        ```
        
    - MapListHandler(多行) -> 把一行紀錄轉換成一個 Map, 多行就是多個 Map, 即 List&lt;Map&gt;
    - ScalarHandler(單行多列) -> 通常用於 select count(*) from stu 語句，結果即是單行單列的，他返回一個 Object
