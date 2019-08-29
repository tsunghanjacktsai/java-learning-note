# 解決全站字符亂碼(POST & GET 中文編碼問題)

## 介紹
亂碼問題:

- 獲取請求參數中的亂碼問題
    - POST 請求: request.setCharacterEncoding("utf-8");
    - GET 請求: new String(request.getParameter("xxx").getBytes("iso-8859-1"), "utf-8");
- 響應的亂碼問題: response.setContextType("text/html;charset=utf-8");

基本上在每個 Servlet 中都要處理亂碼問題，所以應改把這個工作放到過濾器中來完成。

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
    <servlet-name>AServlet</servlet-name>
    <servlet-class>pers.web.servlet.AServlet</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>AServlet</servlet-name>
    <url-pattern>/AServlet</url-pattern>
  </servlet-mapping>
  
  <welcome-file-list>
  	<welcome-file>index.jsp</welcome-file>
  </welcome-file-list>
  
  <filter>
    <filter-name>EncodingFilter</filter-name>
    <filter-class>pers.web.filter.EncodingFilter</filter-class>
  </filter>
  <filter-mapping>
  	<filter-name>EncodingFilter</filter-name>
  	<url-pattern>/*</url-pattern>
  </filter-mapping>
</web-app>

//AServlet.java
package pers.web.servlet;

import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class AServlet extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		String username = request.getParameter("username");
		response.getWriter().println(username);
	}

	public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		String username = request.getParameter("username");
		response.getWriter().println(username);
	}
}

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
	<a href="c:url value='/AServlet?username=張三'/>">點擊這裡</a><br>
	<form action="<c:url value='/AServlet'/>" method="post">
		用戶名: <input type="text" name="username" value="李四">
		<input type="submit" value="提交">
	</form>
  </body>
</html>

//EncodingFilter.java
package pers.web.filter;

import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;

public class EncodingFilter implements Filter {
	@Override
	public void destroy() {
		
	}

	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {
		//處理 post 請求編碼問題
		request.setCharacterEncoding("utf-8");
		
		HttpServletRequest req = (HttpServletRequest) request;
		/*
		 * 處理 GET 請求的編碼問題
		 */
//		String username = request.getParameter("username");
//		username = new String(username.getBytes("ISO-8859-1"), "UTF-8");
		/*
		 * 調包 request
		 * 1. 寫一個 request 的裝飾類
		 * 2. 在放行時，使用我們自己的 request
		 */
		if(req.getMethod().equals("GET")) {
			EncodingRequest er = new EncodingRequest(req);
			chain.doFilter(er, response);
		} else if(req.getMethod().equals("POST")) {
			chain.doFilter(request, response);
		}
	}

	@Override
	public void init(FilterConfig filterConfig) throws ServletException {

	}
}

//EncodingRequest.java
package pers.web.filter;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletRequestWrapper;

/**
 * 裝飾 request
 * @author jack870131
 *
 */
public class EncodingRequest extends HttpServletRequestWrapper {
	private HttpServletRequest req;
	
	public EncodingRequest(HttpServletRequest request) {
		super(request);
		this.req = request;
	}
	
	public String getParameter(String name) {
		String value = req.getParameter(name);
		
		//處理編碼問題
		try {
			value = new String(value.getBytes("iso-8859-1"), "utf-8");
		} catch (Exception e) {
			throw new RuntimeException(e);
		}
		return value;
	}
}
```
