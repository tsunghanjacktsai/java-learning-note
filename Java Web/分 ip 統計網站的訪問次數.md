# 分 ip 統計網站的訪問次數

## 介紹
網站統計每個 IP 地址訪問本網站的次數。
統計工作需要再所有資源之前都執行，那麼就可以放到 Filter 中了。
這個過濾器不打算做攔截操作，是用來做統計的。
用 Map&lt;String, Integer&gt; 來裝載統計的數據，整個網站只需要一個 Map 即可。
使用 ServletContextListener, 在服務器啟動時完成創建，並保存到 ServletContext 中。
Map 保存到 ServletContext 中。

- Map 需要再 Filter 中用來保存數據。
- Map 需要再葉面使用，打印 Map 中的數據。

## Example
```java
//AListener.java
package pers.web.listener;

import java.util.LinkedHashMap;
import java.util.Map;

import javax.servlet.ServletContext;
import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;

public class AListener implements ServletContextListener {
	/**
	 * 在服務器啟動時創建 Map, 保存到 ServletContext
	 */
	@Override
	public void contextInitialized(ServletContextEvent sce) {
		//創建 Map
		Map<String, Integer> map = new LinkedHashMap<String, Integer>();
		//得到 ServletContext
		ServletContext application = sce.getServletContext();
		//把 map 保存到 application 中
		application.setAttribute("map", map);
	}

	@Override
	public void contextDestroyed(ServletContextEvent sce) {
		
	}
}

//AFilter.java
package pers.web.filter;

import java.io.IOException;
import java.util.Map;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;

/**
 * 從 application 中獲取 Map
 * 從 request 中得到當前客戶端的 IP
 * 進行統計工作，結果保存到 Map 中	
 * @author jack870131
 *
 */
public class AFilter implements Filter {
	private FilterConfig config;
	@Override
	public void destroy() {
		
	}

	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {
		/*
		 * 1. 得到 application 中的 map
		 * 2. 從 request 中獲取當前客戶端的 ip 地址
		 * 3. 查看 map 中是否存在這個 ip 對應訪問次數，如果存在，把次數 +1 再保存回去
		 * 4. 如果不存在這個 ip, 那麼說明第一次訪問本站，設置訪問次數為1
		 */
		/*
		 * 1. 得到 application
		 */
		ServletContext app = config.getServletContext();
		Map<String, Integer> map = (Map<String, Integer>) app.getAttribute("map");
		/*
		 * 2. 獲取客戶端的 ip 地址
		 */
		String ip = request.getRemoteAddr();
		/*
		 * 3. 進行判斷
		 */
		if(map.containsKey(ip)) { //這個 ip 在 map 中存在，說明不是第一次 訪問
			int cnt = map.get(ip);
			map.put(ip, cnt + 1);
		} else { //這個 ip 在 map 中不存在，說明是第一次訪問
			map.put(ip, 1);
		}
		app.setAttribute("map", map); //把 map 放回到 app 中
		
		chain.doFilter(request, response); //肯定放行
	}

	/**
	 * 再服務器啟動時就會執行本方法，而且本方法只執行一次
	 */
	@Override
	public void init(FilterConfig fConfig) throws ServletException {
		this.config	= fConfig;
	}
}

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
	<h1>去</h1>
  </body>
</html>

//b.jsp
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
    
    <title>My JSP 'b.jsp' starting page</title>
    
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
	<h1>好</h1>
  </body>
</html>

//show.jsp
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%
	String path = request.getContextPath();
	String basePath = request.getScheme() + "://" + request.getServerName() + ":" + request.getServerPort()
			+ path + "/";
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
	<h1 align="center">顯示結果</h1>
	<table align="center" width="60%" border="1">
		<tr>
			<th>IP</th>
			<th>次數</th>
		</tr>
		<c:forEach items="${applicationScope.map }" var="entry">
			<tr>
				<td>${entry.key }</td>
				<td>${entry.value }</td>
			</tr>
		</c:forEach>
	</table>
</body>
</html>
```
