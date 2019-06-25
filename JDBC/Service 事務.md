# Service 事務

## 介紹
在 Service 中使用 ThreadLocal 來完成事務，為將來學習 Spring 事務打基礎。


## Example
```java
//ReWriteQueryRunner.java
import java.sql.Connection;
import java.sql.SQLException;

import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.ResultSetHandler;

/**
 * 這個類中的方法自己來處理連接的問題，無須外界傳遞
 * 	通過 JdbcUtils.getConnection() 得到連接。有可能是事務連接，也可能是普通連接
 * 	JdbcUtils.releaseConnection() 完成對連接的釋放，如果是普通連接，則關閉
 * @author jack870131
 *
 */
public class ReWriteQueryRunner extends QueryRunner {
	@Override
	public int[] batch(String sql, Object[][] params) throws SQLException {
		/*
		 * 1. 得到連接
		 * 2. 執行父類方法，傳遞連接對象
		 * 3. 釋放連接
		 * 4. 返回值
		 */
		Connection con = JdbcUtils.getConnection();
		int[] result = super.batch(con, sql, params);
		JdbcUtils.releaseConnection(con);
		return result;
	}

	@Override
	public <T> T query(String sql, Object param, ResultSetHandler<T> rsh) throws SQLException {
		Connection con = JdbcUtils.getConnection();
		T result = super.query(con, sql, param, rsh);
		JdbcUtils.releaseConnection(con);
		return result;
	}

	@Override
	public <T> T query(String sql, Object[] params, ResultSetHandler<T> rsh) throws SQLException {
		Connection con = JdbcUtils.getConnection();
		T result = super.query(con, sql, params, rsh);
		JdbcUtils.releaseConnection(con);
		return result;
	}

	@Override
	public <T> T query(String sql, ResultSetHandler<T> rsh, Object... params) throws SQLException {
		Connection con = JdbcUtils.getConnection();
		T result = super.query(con, sql, rsh, params);
		JdbcUtils.releaseConnection(con);
		return result;
	}

	@Override
	public <T> T query(String sql, ResultSetHandler<T> rsh) throws SQLException {
		Connection con = JdbcUtils.getConnection();
		T result = super.query(con, sql, rsh);
		JdbcUtils.releaseConnection(con);
		return result;
	}

	@Override
	public int update(String sql) throws SQLException {
		Connection con = JdbcUtils.getConnection();
		int result = super.update(con, sql);
		JdbcUtils.releaseConnection(con);
		return result;
	}

	@Override
	public int update(String sql, Object param) throws SQLException {
		Connection con = JdbcUtils.getConnection();
		int result = super.update(con, sql, param);
		JdbcUtils.releaseConnection(con);
		return result;
	}

	@Override
	public int update(String sql, Object... params) throws SQLException {
		Connection con = JdbcUtils.getConnection();
		int result = super.update(con, sql, params);
		JdbcUtils.releaseConnection(con);
		return result;
	}
}

//JdbcUtils.java
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
/**
 * @author jack870131
 *
 */
public class JdbcUtils {
	//使用文件的默認配置，要求必須給出 c3p0-config.xml 
	private static ComboPooledDataSource dataSource = new ComboPooledDataSource();
	//事務專用連接
	private static ThreadLocal<Connection> tl = new ThreadLocal<>();
	
	/**
	 * 使用連接池返回一個連接對象
	 * @return
	 * @throws SQLException
	 */
	public static Connection getConnection() throws SQLException {
		Connection con = tl.get();
		//當 con 不等於 null, 說明已經調用 beginTransaction(), 表示開啟了事務
		if(con != null) return con;
		return dataSource.getConnection();
	}
	
	/**
	 * 返回連接池對象
	 * @return
	 */
	public static DataSource getDataSource() {
		return dataSource;
	}
	
	/**
	 * 開啟事務
	 * 1. 獲取一個 Connection, 設置他的 setAutoCommit(false)
	 * 2. 保證 dao 中使用的連接是我們剛剛創建的
	 * ---------------------------------------
	 * 1. 創建 Connection, 設置為手動提交
	 * 2. 把這個 Connection 給 dao 用
	 * 3. 還要讓 commitTransaction 或  rollbackTransaction 可以獲取到
	 * @throws SQLException 
	 */
	/**
	 * @throws SQLException
	 */
	public static void beginTransaction() throws SQLException {
		Connection con = tl.get();
		if(con != null) throw new SQLException("已經開啟事務，就不要重複開啟了!");
		/*
		 * 1. 給 con 賦值
		 * 2. 給 con 設置為手動提交
		 */
		con = getConnection();//給 con 賦值，表示事務已經開始
		con.setAutoCommit(false);
		
		tl.set(con); //把當前線程的連接保存起來
	}
	
	/**
	 * 提交事務
	 * 1. 獲取 beginTransaction 提供的 Connection, 然後調用 commit 方法
	 * @throws SQLException 
	 */
	public static void commitTransaction() throws SQLException {
		Connection con = tl.get(); //獲取當前線程的連接
		if(con == null) throw new SQLException("還沒有開啟事務，不能提交");
		/*
		 * 1. 直接使用 con.commit()
		 */
		con.commit();
		con.close();
		//把他設置為 null, 表示事務已經結束，下次再去調用 getConnection() 返回的就不是 con 了
		tl.remove(); //從 tl 中移除連接
	}
	
	/**
	 * 提交事務
	 * 1. 獲取 beginTransaction 提供的 Connection, 然後調用 rollback 方法
	 * @throws SQLException 
	 */
	public static void rollBackTransaction() throws SQLException {
		Connection con = tl.get();
		if(con == null) throw new SQLException("還沒有開啟事務，不能回滾");
		/*
		 * 1. 直接使用 con.rollback()
		 */
		con.rollback();
		con.close();
		con = null;
		tl.remove();
	}
	
	/**
	 * 釋放連接
	 * @param connection
	 * @throws SQLException 
	 */
	public static void releaseConnection(Connection connection) throws SQLException {
		Connection con = tl.get();
		/*
		 * 判斷他是不是事務，如果是，就不關閉
		 * 如果不是事務專用，那麼就關閉
		 */
		//如果 con == null, 說明現在沒有事務，那麼 connection 一定不是事務專用的
		if(con == null) connection.close();
		//如果 con != null, 說明有事務，那麼需要判斷參數連接是否與 con 相等，若不等，說明參數連接不是事務專用連接
		if(con != connection) connection.close(); 
	}
}

//AccountDao.java
import java.sql.SQLException;

import org.apache.commons.dbutils.QueryRunner;

public class AccountDao {
	public static void update(String name, double money) throws SQLException {
		QueryRunner qr = new ReWriteQueryRunner();
		String sql = "update account set balance=balance+? where name=?";
		Object[] params = {money, name};
		//我們需要自己提供連接
		qr.update(sql, params);
	}
}

//AService.java
import java.sql.SQLException;

import org.junit.Test;

public class AService {
	private AccountDao dao = new AccountDao();
	
	@Test
	public void serviceMethod() throws Exception {
		try {
			JdbcUtils.beginTransaction();
			
			dao.update("Jack", 1000);
			
			if(true) throw new RuntimeException();
			
			dao.update("Vivian", 1000);
			
			JdbcUtils.commitTransaction();
		} catch(Exception e) {
			try {
				JdbcUtils.rollBackTransaction();
			} catch (SQLException e1) {
			}
			
			throw e;
		}
	}
}
```
