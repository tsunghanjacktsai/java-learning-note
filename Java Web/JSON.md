# JSON

## 介紹
- 他是 js 提供的一種數據交換格式

## 語法
- **{}**: 是對象
    - 屬性名必須使用雙引號刮起來，單引不行
    - 屬性值:
        - null
        - 數值
        - 字符串
        - 數組: 使用 [] 刮起來
        - boolean 值: true 和 false
        

## 應用 json
- var person = {"name":"jack", "age":20, "sex":"male"}

## JSON 與 XML 比較
- 可讀性: XML 勝出
- 解碼難度: JSON 本身就是 JS 對象，簡單很多
- 流行度: XML 已經流行很多年，但在 AJAX 領域，JSON 更受歡迎。

## JSON-LIB
- 他可以把 javabean 轉換成 json 串
- 導 jar 包
- 核心類
    - JSONObject -> Map
        - toString();
        - JSONObject map = JSONObject.fromObject(person); 把對象轉換成 JSONObject 對象
    - JSONArray -> List
        - toString();
        - JSONArray jsonArray = JSONObject.fromObject(list); 把 list 轉換成 JSONArray 對象
        

## Example
```html
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

//json2.jsp<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
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

<title>My JSP 'json2.jsp' starting page</title>

<meta http-equiv="pragma" content="no-cache">
<meta http-equiv="cache-control" content="no-cache">
<meta http-equiv="expires" content="0">
<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
<meta http-equiv="description" content="This is my page">
<!--
	<link rel="stylesheet" type="text/css" href="styles.css">
	-->
<script type="text/javascript">
	function createXMLHttpRequest() {
		try {
			return new XMLHttpRequest(); //大多數瀏覽器
		} catch(e) {
			try {
				return ActiveXObject("Msxml2.XMLHTTP"); //IE6.0
			} catch(e) {
				try {
					return ActiveXObject("Microsoft.XMLHTTP"); //IE5.5
				} catch(e) {
					alert("What web explorer you are using");
					throw e;
				}
			}
		}
	}
	
	window.onload = function() {
		//獲取 btn 元素
		var btn = document.getElementById("btn");
		btn.onclick = function() { //給按紐的點擊事件添加監聽
			//使用 ajax 得到服務器端響應，把結果顯示到 h3 中
			//1. 得到 request
			var xmlHttp = new createXMLHttpRequest();
			//2. 連接
			xmlHttp.open("GET", "<c:url value='/AServlet'/>", true);
			//3. 發送
			xmlHttp.send(null);
			//4. 給 xmlHttp 的狀態改變事件上添加監聽
			xmlHttp.onreadystatechange = function() {
				//雙重判斷
				if(xmlHttp.readyState == 4 && xmlHttp.status == 200) {
					var text = xmlHttp.responseText; //他是一個 json 串
					//執行 json 串
					var person = eval("(" + text + ")");
					var s = person.name + ", " + person.age + ", " + person.sex;
					document.getElementById("h3").innerHTML = s;
				}
			};
		};
	};
</script>
</head>

<body>
	<%--點擊按鈕後，把服務器響應的數據顯示到 h3 元素中 --%>
	<button id="btn">點擊這裡</button>
	<h1>JSON之Hello World</h1>
	<h3 id="h3"></h3>
</body>
</html>
```

### JSON-LIB
```java
package pers.demo1;

import java.util.ArrayList;
import java.util.List;

import org.junit.Test;

import net.sf.json.JSONArray;
import net.sf.json.JSONObject;

/**
 * 演示 JSON-LIB 小工具
 * @author jack870131
 *
 */
public class Demo1 {
	@Test
	public void fun1() {
		JSONObject map = new JSONObject();
		map.put("name", "Jack");
		map.put("age", 20);
		map.put("sex", "male");
		
		String s = map.toString();
		System.out.println(s);
	}
	
	/*
	 * 當你已經有一個 person 對象時，可以把 Person 轉換成 JSONObject 對象
	 */
	@Test
	public void fun2() {
		Person p = new Person("Jack", 20, "Male");
		//把對象轉換成 JSONObject 類型
		JSONObject map = JSONObject.fromObject(p);
		System.out.println(map.toString());
	}
	
	/*
	 * JSONArray
	 */
	@Test
	public void fun3() {
		Person p1 = new Person("Vivian", 21, "Female");
		Person p2 = new Person("Jack", 20, "Male");
		
		JSONArray list = new JSONArray();
		list.add(p1);
		list.add(p2);
		
		System.out.println(list.toString());
	}

	/*
	 * 原來就有一個 List, 我們需要把 List 轉換成 JSONArray
	 */
	@Test
	public void fun4() {
		Person p1 = new Person("Vivian", 21, "Female");
		Person p2 = new Person("Jack", 20, "Male");
		List<Person> list = new ArrayList<Person>();
		list.add(p1);
		list.add(p2);
		
		System.out.println(JSONArray.fromObject(list).toString());
	}
}
```
