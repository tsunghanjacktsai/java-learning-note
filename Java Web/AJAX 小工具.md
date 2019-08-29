# AJAX 小工具

## Example
```java
//web.xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>Java_Web</display-name>
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
</web-app>

//AServlet.java
package pers.web.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class AServlet extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		/*
		 * 向客戶端發送 json 串
		 */
		String str = "{\"name\":\"Jack\", \"age\":20, \"sex\":\"male\"}";
		response.getWriter().print(str);
		System.out.println(str);
	}
}

//json3.jsp
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%
	String path = request.getContextPath();
	String basePath = request.getScheme() + "://" + request.getServerName() + ":" + request.getServerPort()
			+ path + "/";
%>

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
<head>
<base href="<%=basePath%>">

<title>My JSP 'json3.jsp' starting page</title>

<meta http-equiv="pragma" content="no-cache">
<meta http-equiv="cache-control" content="no-cache">
<meta http-equiv="expires" content="0">
<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
<meta http-equiv="description" content="This is my page">
<!--
	<link rel="stylesheet" type="text/css" href="styles.css">
	-->
<script type="text/javascript" src="<c:url value='/ajax-lib/ajaxutils.js'/>"></script>
	
<script type="text/javascript">
	window.onload = function() {
		var btn = document.getElementById("btn");
		btn.onclick = function() {
			/*
			1. ajax
			*/
			ajax(
				{
					url : "<c:url value='/AServlet'/>",
					type : "json",
					callback : function(data) {
						document.getElementById("h3").innerHTML = data.name + ", " + data.age + ", " + data.sex;
					}
				}
			);
		};
	};
</script>
</head>

<body>
	<button id="btn">點擊這裡</button>
	<h1>演示ajax-lib</h1>
	<h3 id="h3"></h3>
</body>
</html>

//ajaxutils.js
//創建 request 對象
function createXMLHttpRequest() {
	try {
		return new XMLHttpRequest(); //大多數瀏覽器
	} catch (e) {
		try {
			return ActiveXObject("Msxml2.XMLHTTP"); //IE6.0
		} catch (e) {
			try {
				return ActiveXObject("Microsoft.XMLHTTP"); //IE5.5
			} catch (e) {
				alert("What web explorer you are using?");
				throw e;
			}
		}
	}
}

/*
 * option 對象有如下屬性:
 */
/*請求方式*/ method,
/*請求的 url*/ url,
/*是否異步*/ asyn,
/*請求體*/ params,
/*回調方法*/ callback,
/*服務器響應數據轉換成甚麼類型*/ type
function ajax(option) {
	/*
	 * 1. 得到 xmlHttp
	 */
	var xmlHttp = createXMLHttpRequest();
	/*
	 * 2. 打開連接
	 */
	if (!option.method) { //默認為 GET 請求	
		option.method = "GET";
	}
	if (option.asyn == undefined) { //默認為異步處理
		option.asyn = true;
	}
	xmlHttp.open(option.method, option.url, option.asyn);
	/*
	 * 3. 判斷是否為 POST
	 */
	if ("POST" == option.method) {
		xmlHttp.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
	}
	/*
	 * 4. 發送請求
	 */
	xmlHttp.send(option.params);
	/*
	 * 5. 註冊監聽
	 */
	xmlHttp.onreadystatechange = function() {
		if (xmlHttp.readyState == 4 && xmlHttp.status == 200) { //雙重判斷
			var data;
			//獲取服務器的響應數據，進行轉換
			if (!option.type) { //如果 type 沒有賦值，那麼默認為文本
				data = xmlHttp.responseText;
			} else if (option.type == "xml") {
				data == xmlHttp.responseXML;
			} else if (option.type == "text") {
				data = xmlHttp.responseText;
			} else if (option.type == "json") {
				var text = xmlHttp.responseText;
				data = eval("(" + text + ")");
			}

			//調用回調方法
			option.callback(data);
		}
	};
};
```
