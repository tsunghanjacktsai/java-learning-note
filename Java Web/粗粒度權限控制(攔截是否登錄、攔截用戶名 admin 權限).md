# 粗粒度權限控制(攔截是否登錄、攔截用戶名 admin 權限)

## 介紹
給出三個頁面: index.jsp、user.jsp、admin.jsp

- index.jsp: 誰都可以訪問，沒有限制
- user.jsp: 只有登錄用戶才能訪問。
- admin.jsp: 只有管理員才能訪問。

## Example
```Java
//web.xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.5"
		xmlns="http://java.sun.com/xml/ns/javaee"
		xmlns:xsi="http:www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
		http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">
  <servlet>
    <servlet-name>LoginServlet</servlet-name>
    <servlet-class>pers.web.servlet.LoginServlet</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>LoginServlet</servlet-name>
    <url-pattern>/LoginServlet</url-pattern>
  </servlet-mapping>
  <welcome-file-list>
  	<welcome-file>index.jsp</welcome-file>
  </welcome-file-list>
  
  <filter>
  	<filter-name>UserFilter</filter-name>
  	<filter-class>pers.web.filter.UserFilter</filter-class>
  </filter>
  <filter-mapping>
  	<filter-name>UserFilter</filter-name>
  	<url-pattern>/users/*</url-pattern>
  </filter-mapping>
  <filter>
  	<filter-name>AdminFilter</filter-name>
  	<filter-class>pers.web.filter.AdminFilter</filter-class>
  </filter>
  <filter-mapping>
  	<filter-name>AdminFilter</filter-name>
  	<url-pattern>/admin/*</url-pattern>
  </filter-mapping>
</web-app>

//a.jsp
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <base href="<%=basePath%>">
    
    <title>My JSP 'a.jsp' starting page</title>
    
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
	<h1>你好，管理員</h1>
	<a href="<c:url value='/index.jsp'/>">遊客入口</a><br>
	<a href="<c:url value='/users/u.jsp'/>">會員入口</a><br>
	<a href="<c:url value='/admin/a.jsp'/>">管理員入口</a><br>
  </body>
</html>

//u.jsp<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <base href="<%=basePath%>">
    
    <title>My JSP 'a.jsp' starting page</title>
    
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
	<h1>你好，會員</h1>
	<a href="<c:url value='/index.jsp'/>">遊客入口</a><br>
	<a href="<c:url value='/users/u.jsp'/>">會員入口</a><br>
	<a href="<c:url value='/admin/a.jsp'/>">管理員入口</a><br>
  </body>
</html>

//index.jsp
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
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
	<h1>你好，遊客</h1>
	<a href="<c:url value='/index.jsp'/>">遊客入口</a><br>
	<a href="<c:url value='/users/u.jsp'/>">會員入口</a><br>
	<a href="<c:url value='/admin/a.jsp'/>">管理員入口</a><br>
  </body>
</html>

//login.jsp
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <base href="<%=basePath%>">
    
    <title>My JSP 'login.jsp' starting page</title>
    
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
	<h1>登錄</h1>
	${msg }
	<form action="<c:url value='/LoginServlet'/>" method="post">
		用戶名: <input type="text" name="username">
		<input type="submit" value="登錄">
	</form>
  </body>
</html>

//LoginServlet.java
package pers.web.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class LoginServlet extends HttpServlet {

	public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		/*
		 * 1. 獲取用戶名
		 * 2. 判斷用戶名是否包含 jack
		 * 3. 如果包含，就是管理員
		 * 4. 如果不包含，就是普通會員
		 * 5. 要把登錄的用戶名保存到 session 中
		 * 6. 轉發到 index.jsp
		 */
		String username = request.getParameter("username");
		if(username.contains("jack")) {
			request.getSession().setAttribute("admin", username);
		} else {
			request.getSession().setAttribute("username", username);
		}
		request.getRequestDispatcher("/index.jsp").forward(request, response);
	}
}

//UserFilter.java
	@Override
	public void destroy() {

	}

	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {
		/*
		 * 1. 得到 session
		 * 2. 判斷 session 域中是否存在 admin, 如果存在, 放行
		 * 3. 判斷 session 域中是否存在 username, 如果存在, 放行, 否則打回到 login.jsp
		 */
		HttpServletRequest req = (HttpServletRequest) request;
		String name = (String) req.getSession().getAttribute("admin");
		if(name != null) {
			chain.doFilter(request, response);
			return;
		}
		name = (String)req.getSession().getAttribute("username");
		if(name == null) {
			chain.doFilter(request, response);
 		} else {
 			req.setAttribute("msg", "你什麼都不是");
 			req.getRequestDispatcher("/login.jsp").forward(request, response);	
 		}
	}

	@Override
	public void init(FilterConfig filterConfig) throws ServletException {

	}
}

//AdminFilter.java
package pers.web.filter;

import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;

public class AdminFilter implements Filter {
	@Override
	public void destroy() {

	}

	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {
		/*
		 * 1. 得到 session
		 * 2. 判斷 session 域中是否存在 admin, 如果存在, 放行
		 * 3. 判斷 session 域中是否存在 username, 如果存在, 放行, 否則打回到 login.jsp
		 */
		HttpServletRequest req = (HttpServletRequest) request;
		String name = (String) req.getSession().getAttribute("admin");
		if(name != null) {
			chain.doFilter(request, response);
			return;
		} else {
 			req.setAttribute("msg", "你肯定不是管理員");
 			req.getRequestDispatcher("/login.jsp").forward(request, response);	
 		}
	}

	@Override
	public void init(FilterConfig filterConfig) throws ServletException {

	}
}
```
