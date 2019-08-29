# JDBC 對象介紹

## DriverManager
```java
Class.forName("com.mysql.jdbc.Driver"); //註冊驅動
String url = "jdbc:mysql://localhost:3306/mydb1";
String username = "root";
Stirng password = "xxxxx";
Connection con = DriverManager.getConnection(url, username, password);
```
**注意:** 上面的代碼可能出現的兩種異常。
- ClassNotFoundException: 這個異常是在第1句上出現的，出現這個異常有兩個可能。
    - 沒有給出 mysql 的 jar 包。
    - 查看類型是不是 com.mysql.jdbc.Driver
- SQLException: 這個異常出現在第5句，出現這個異常就是三個參數的問題，往往 username, password 不會出錯，所以需要查看 url 是否輸入錯誤。

## Connection
Connection 最重要的方法就是**獲取 Statement**。
- Statement stmt = con.createStatement();
下面兩個 int 參數用來確定創建的 Statement 能生成什麼樣的結果集。
- Statement stmt = con.createStatement(int, int);

## Statement
- int executeUpdate(String sql): 執行更新操作，集執行 insert, update, delete 語句，其實這個方法也可以執行 create table, alter table, drop table 等語句，但很少會使用 jdbc 來執行這些語句。
- ResultSet executeQuery(String sql): 執行查詢操作，執行查詢操作會返回 ResultSet, 集結果集。
- boolean(execute): 這個方法可以用來執行增、刪、改、查所有SQL語句。該方法返回的是 boolean 類型，表示 SQL 語句是否執行成功。
如果使用 execute() 方法執行的是更新語句，還要調用 int getUpdateCount() 來獲取 insert、update、delete 語句所影響的行數。
如果使用 execute() 方法執行的是查詢語句，那麼還要調用 ResultSet getResultSet() 來獲取 select 語句的查詢結果。

## ResultSet 之滾動結果集
ResultSet 表示結果集，他是一個二維的表格。ResultSet 內部戶一個行光標(游標)，ResultSet 有一系列的方法來移動光標:
- void beforeFirst(): 把光標放到第一行的前面，這也是光標默認的位置。
- void afterLast(); 把光標放到最後一行的後面。
- boolean first(): 把光標放到第一行的位置上，返回值表示調控光標是否成功。
- boolean last(): 把光標放到最後一行的位置上。
- boolean isBeforeFirst(): 當前光標位置是否在第一行前面。
- boolean isAfterLast(): 當前光標位置是否在最後一行的後面。
- bolean isFirst(); 當前光標位置是否在第一行上。
- boolean isLast(): 當前光標位置是否在最後一行上。

獲取結果集元數據
- 得到元數據: rs.getMetaData(), 返回值為 ResultSetMetaData;
- 獲取結果集列數: int getColumnCount();
- 獲取指定列的列名: String getColumnName(int colindex);

結果集特性: 當使用 Connection 的 createStatement 時，已經確定了 Statement 生成的結果集是什麼特性。
- 是否可滾動
- 是否敏感
- 是否可更新
