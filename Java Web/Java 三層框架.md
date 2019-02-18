# Java 三層框架

## 框架
- Web 層 -> 與 Web 相關的內容(Servlet, JSP, Servlet 相關 API: request, response, session, ServletContext)
- 業務層 -> 業務對象(Service)
- 數據層 -> 操作數據庫(DAO Data Access Object)(所有對數據庫的操作，不能跳出到 DAO 之外)
![](https://github.com/jack870131/Markdown-Pic/blob/master/Picture/Java%20%E4%B8%89%E5%B1%A4%E6%A1%86%E6%9E%B6.png)

## Example
```java
//User.java
package domain;

/**
 * 把數據庫中查詢的結果保存到這個對象中
 * @author jack870131
 *
 */
public class User {
	private String username;
	private String password;
	public User(String username, String password) {
		super();
		this.username = username;
		this.password = password;
	}
	public String getUsername() {
		return username;
	}
	public void setUsername(String username) {
		this.username = username;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
	@Override
	public String toString() {
		return "User [username=" + username + ", password=" + password + "]";
	}
}

//UserDao.java
package dao;

import domain.User;

public class UserDao {
	/*
	 * 把 xml 中的數據查詢出來後，封裝到 user 對象中，然後返回
	 */
	public User find() {
		return new User("Jack", "00000");
	}
}

//UserService.java
package service;

import dao.UserDao;
import domain.User;

public class UserService {
	/*
	 * service 層依賴 dao 層
	 */
	private UserDao userDao = new UserDao();
	/*
	 * service 的查詢需要使用 dao 來完成
	 */
	public User find() {
		return userDao.find();
	}
}

//UserServlet.java
package web.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import domain.User;
import service.UserService;

public class UserServlet extends HttpServlet {

	public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		/*
		 * 在 Servlet 中依賴 service, 然後通過 service 完成功能，把結果保存到 request 中
		 * 轉發到 jsp 顯示
		 */
		UserService userService = new UserService();
		User user = userService.find();
		
		request.setAttribute("user", user);
		
		request.getRequestDispatcher("/show.jsp").forward(request, response);
	}
}

//index.jsp
<%@ page language="java" import="java.util.*" pageEncoding="BIG5"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <base href="<%=basePath%>">
    
    <title>My JSP 'index.jsp' starting page</title>
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">    
	<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
	<meta http-equiv="description" content="This is my page">
	<!--
	<link rel="stylesheet" type="text/css" href="styles.css">
	-->
  </head>
  
  <body>
  	<a href="<c:url value='/UserServlet'/>">點擊這裡</a>
  </body>
</html>

//show.jsp
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <base href="<%=basePath%>">
    
    <title>My JSP 'show.jsp' starting page</title>
    
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">    
	<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
	<meta http-equiv="description" content="This is my page">
	<!--
	<link rel="stylesheet" type="text/css" href="styles.css">
	-->

  </head>
  
  <body>
  	用戶名: ${user.username }<br>
  	密碼: ${user.password }
  </body>
</html>
```
結果
```
用戶名: Jack
密碼: 00000
```

