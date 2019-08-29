# JdbcUtils 工具類

## V1.0
```java
package demo3;

import java.io.IOException;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.Properties;

public class JdbcUtils {
	private static Properties props = null;
	
	//只在 JdbcUtils 類被加載時執行一次
	static {
		//給 props 進行初始化，即加載 dbconfig.properties 文件到 props 對象中
		try {
			InputStream in = JdbcUtils.class.getClassLoader()
					.getResourceAsStream("dbconfig.properties");
			Properties props = new Properties();
				props.load(in);
		} catch (IOException e) {
			throw new RuntimeException(e);
		}
		
		//加載驅動類
		try {
			Class.forName(props.getProperty("driverClassName"));
		} catch (ClassNotFoundException e) {
			throw new RuntimeException(e);
		}
	}
	
	//獲取連接
	public static Connection getConnection() throws SQLException {
		/*
		 * 1. 加載配置文件
		 * 2. 加載驅動類
		 * 3. 調用 DriverManager.getConnection()
		 */
		//得到 Connection
		return DriverManager.getConnection(props.getProperty("url"), 
				props.getProperty("username"), 
				props.getProperty("password"));
	}
}
```
---
## V2.0
```
package demo8;

import java.io.IOException;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.Properties;

import javax.sql.DataSource;

import com.mchange.v2.c3p0.ComboPooledDataSource;

/**
 * JdbcUtils v2.0
 * @author jack870131
 *
 */
public class JdbcUtils {
	//使用文件的默認配置，要求必須給出 c3p0-config.xml 
	private static ComboPooledDataSource dataSource = new ComboPooledDataSource();
	
	/**
	 * 使用連接池返回一個連接對象
	 * @return
	 * @throws SQLException
	 */
	public static Connection getConnection() throws SQLException {
		return dataSource.getConnection();
	}
	
	/**
	 * 返回連接池對象
	 * @return
	 */
	public static DataSource getDataSource() {
		return dataSource;
	}
}
```
