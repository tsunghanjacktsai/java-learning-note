# AJAX

## 介紹
- AJAX (Asynchronous Javascript And Xml)，異步的 js & xml。
- 他能使用 js 訪問服務器，而且是異步訪問。
- 服務器給客戶端的響應一般是整個頁面，一個 html 完整頁面。但在 ajax 中因為是局部刷新，那麼服務器就不用再響應整個頁面。而只是數據。
- text: 純文本
- xml 
- json: 他是 js 提供的數據交互額是，他在 ajax 中最受歡迎。
<br>

![](https://github.com/jack870131/Markdown-Pic/blob/master/Picture/ajax1.png?raw=true)

## 異步交互和同步交互
- 同步:
    - 發一個請求，就要等待服務器的響應結束，然後才能發第二個請求。
    - 刷新的就是整個頁面。
- 異步:
    - 發一個請求，無須等待服務器的響應，然後就可以發第二個請求。
    - 可以使用 js 接收服務器的響應，然後使用 js 來**局部刷新**。
    

## AJAX 的優缺點
優點:
- 異步交互: 增強用戶的體驗。
- 性能: 因為服務器無須再響應整個頁面，只需要響應部分內容，所以服務器的壓力減小。

缺點:
- ajax 不能應用在所有場景
- ajax 無端增多了對服務器的訪問次數，給服務器帶來壓力。


## AJAX 發送異步請求(四步操作)
- 第一步(得到 XMLHttpRequest)
    - 大多數瀏覽器都支持: var xmlHttp = new XMLHttpRequest(); (IE6.0 以前不支持)
    - 編寫創建 XMLHttpRequest 對象的函數

        ```javascript
    function createXMLHttpRequest() {
        try {
            return new XMLHttpRequest();
        } catch(e) {
            try {
                return new ActiveXObject("Msxml2.XMLHTTP"); //IE6.0
            } catch(e) {
                try {
                    return new ActiveXObject("Microsoft.XMLHTTP"); //IE5.5以前
                } catch(e) {
                    throw e;
                }
            }
        }
    }
    ```

- 第二步(打開與服務器的連接)
    - xmlHttp.open(): 用來打開與服務器的連接，他需要三個參數。
        - 請求方式: 可以是 GET 或 POST
        - 請求的URL: 指定服務器端資源，例如: /Java_Web/AServlet
        - 請求是否為異步: 如果為 true 表示發送異步請求，否則同步請求。
    - xmlHttp.open("GET", "/Java_Web/AServlet", true);
- 第三步(發送請求)
    - xmlHttp.send(null): 如果不給可能會造成部分瀏覽器無法發送。
        - 參數: 就是請求體內容，如果是 GET 請求，必須給出 null。
- 第四步()
    - 在 xmlHttp 對象的一個事件上註冊監聽器: onreadystatechange
    - xmlHttp 對象一共有5個狀態:
        - 0狀態: 剛創建，還沒有調用 open() 方法。
        - 1狀態: 請求開始，調用了 open() 方法，但還沒有調用 send() 方法
        - 2狀態: 調用完了 send() 方法。
        - 3狀態: 服務器已經開始響應，但不表示響應結束。
        - 4狀態: 服務器響應結束。(通常我們只關心最後這個狀態)
    - 得到 xmlHttp 對象的狀態
        - var state = xmlHttp.readyState; //可能是 0, 1, 2, 3, 4
    - 得到服務器響應的狀態碼
        - var status = xmlHttp.status; //例如為 200, 404, 500
        - var content = xmlHttp.responseText; //得到服務器響應的文本格式的內容
        - var content = xmlHttp.responseXML; //得到服務器響應的 xml 響應的內容，他是 Document 對象
        
        ```
        xmlHttp.onreadysstatechange = function() { //xmlHttp 的5種狀態都會調用本方法
            if(xmlHttp.readyState == 4 && xmlHttp.status == 200) { //雙重判斷: 判斷是否為4狀態，而且還要判斷是否為200
                var text = xmlHttp.responseText;
            }
        }
        ```


## Example
### Hello AJAX
```java
//web.xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>Java_Web</display-name>
  <servlet>
    <servlet-name>AServlet</servlet-name>
    <servlet-class>pers.servlet.AServlet</servlet-class>
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
package pers.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class AServlet extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("Hello AJAX");
		response.getWriter().print("Hello AJAX");
	}
}

//ajax1.jsp
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
    
    <title>My JSP 'ajax1.jsp' starting page</title>
    
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">    
	<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
	<meta http-equiv="description" content="This is my page">
	<!--
	<link rel="stylesheet" type="text/css" href="styles.css">
	-->
	<script type="text/javascript">
		//創建異步對象
		function createXMLHttpRequest() {
			try {
				return new XMLHttpRequest(); 
			} catch(e) {
				try {
					return ActiveXObject("Msxml2.XMLHTTP");
				} catch(e) {
					try {
						return ActiveXObject("Microsoft.XMLHTTP");	
					} catch(e) {
						alert("What web explorer you are using?");
						throw e;
					}
				}
			} 
		}
		
		window.onload = function() { //文本加載完畢後執行
			var btn = document.getElementById("btn");
			btn.onclick = function() { //給按鈕的點擊事件註冊監聽
				/*
				ajax 四步操作，得到服務器的響應
				把響應結果顯示到 h1 元素中
				*/
				/*
				1. 得到異步對象
				*/
				 var xmlHttp = createXMLHttpRequest();
				/*
				2. 打開與服務器的連接
				 - 指定請求方式
				 - 指定請求的 URL
				 - 指定是否為異步請求
				*/
				 xmlHttp.open("GET", "<c:url value='/AServlet'/>", true);
				/*
				3. 發送請求
				*/
				 xmlHttp.send(null); //GET 請求沒有請求體，但也要給出 null, 不然 FireFox 可能會不能發送
				 /*
				 4. 給異步對象的 onreadystatechange 事件註冊監聽器
				 */
				  xmlHttp.onreadystatechange = function() { //當 xmlHttp 的狀態發生變化時執行
					 	//雙重判斷: xmlHttp 的狀態為4(服務器響應結束), 以及服務器響應的狀態碼為200(響應成功)
						if(xmlHttp.readyState == 4 && xmlHttp.status == 200) {
							 //獲取服務器的響應結果
							 var text = xmlHttp.responseText;
							 //獲取 h1 元素
							 var h1 = document.getElementById("h1");
							 h1.innerHTML = text;
						}
				 };
			};
		};
	</script>
  </head>
  
  <body>
	<button id="btn">點擊這裡</button>
	<h1 id="h1"></h1>
  </body>
</html>
```


### 發送 POST 請求(如果發送請求時需要帶有參數，一般都用 POST 請求)
- open: xmlHttp.open("POST"..._);
- 添加一步: 設置 Content-Type 請求頭:
    - xmlHttp.setRequestHeader("Content-Type", "application/x-www.form-urlencoded");
- send: xmlHttp.send("username=jack870131&password=123"); //發送請求時指定請求體

```
//web.xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>Java_Web</display-name>
  <servlet>
    <servlet-name>AServlet</servlet-name>
    <servlet-class>pers.servlet.AServlet</servlet-class>
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
package pers.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class AServlet extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("Hello AJAX");
		response.getWriter().print("Hello AJAX");
	}
	
	@Override
	public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		String username = request.getParameter("username"); //獲取請求參數
		System.out.println("(POST: ) Hello AJAX" + username);
		response.getWriter().print("(POST: ) Hello AJAX" + username);
	}
}

//ajax2.jsp
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

<title>My JSP 'ajax1.jsp' starting page</title>

<meta http-equiv="pragma" content="no-cache">
<meta http-equiv="cache-control" content="no-cache">
<meta http-equiv="expires" content="0">
<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
<meta http-equiv="description" content="This is my page">
<!--
	<link rel="stylesheet" type="text/css" href="styles.css">
	-->
<script type="text/javascript">
	//創建異步對象
	function createXMLHttpRequest() {
		try {
			return new XMLHttpRequest();
		} catch (e) {
			try {
				return ActiveXObject("Msxml2.XMLHTTP");
			} catch (e) {
				try {
					return ActiveXObject("Microsoft.XMLHTTP");
				} catch (e) {
					alert("What web explorer you are using?");
					throw e;
				}
			}
		}
	}

	window.onload = function() { //文本加載完畢後執行
		var btn = document.getElementById("btn");
		btn.onclick = function() { //給按鈕的點擊事件註冊監聽
			/*
			ajax 四步操作，得到服務器的響應
			把響應結果顯示到 h1 元素中
			*/
			/*
			1. 得到異步對象
			*/
			var xmlHttp = createXMLHttpRequest();
			/*
			2. 打開與服務器的連接
			 - 指定請求方式
			 - 指定請求的 URL
			 - 指定是否為異步請求
			*/
			//修改 open 方法，指定請求方式為 POST
			xmlHttp.open("POST", "<c:url value='/AServlet'/>", true);
			//設置請求頭: Content-Type
			xmlHttp.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
			/*
			3. 發送請求
			*/
			//發送時指定請求體
			xmlHttp.send("username=jack870131&password=123"); //GET 請求沒有請求體，但也要給出 null, 不然 FireFox 可能會不能發送
			/*
			4. 給異步對象的 onreadystatechange 事件註冊監聽器
			*/
			xmlHttp.onreadystatechange = function() { //當 xmlHttp 的狀態發生變化時執行
				//雙重判斷: xmlHttp 的狀態為4(服務器響應結束), 以及服務器響應的狀態碼為200(響應成功)
				if (xmlHttp.readyState == 4 && xmlHttp.status == 200) {
					//獲取服務器的響應結果
					var text = xmlHttp.responseText;
					//獲取 h1 元素
					var h1 = document.getElementById("h1");
					h1.innerHTML = text;
				}
			};
		};
	};
</script>
</head>

<body>
	<button id="btn">點擊這裡</button>
	<h1 id="h1"></h1>
</body>
</html>
```


### 註冊表單之校驗用戶是否註冊
- 編寫頁面:
    - ajax3.jsp
        - 給出註冊表單頁面
        - 給用戶名文本框添加 onblur 事件的監聽
        - 獲取文本框的內容，通過 ajax 4步發送給服務器，得到響應結果
            - 如果為1: 在文本框後顯示"用戶名已被註冊"
            - 如果為0: 什麼都不做
- 編寫 Servlet
    - ValidateUsernameServlet
        - 獲取客戶端傳遞的用戶名參數
        - 判斷是否為 jack870131
            - 是: 返回1
            - 否: 返回0
            
```java
//web.xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>Java_Web</display-name>
  <servlet>
    <servlet-name>ValidateUsernameServlet</servlet-name>
    <servlet-class>pers.servlet.ValidateUsernameServlet</servlet-class>
  </servlet>

  <servlet-mapping>
    <servlet-name>ValidateUsernameServlet</servlet-name>
    <url-pattern>/ValidateUsernameServlet</url-pattern>
  </servlet-mapping>
  <welcome-file-list>
    <welcome-file>index.jsp</welcome-file>
  </welcome-file-list>
</web-app>

//ValidateUsernameServlet.java
package pers.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class ValidateUsernameServlet extends HttpServlet {
	public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setCharacterEncoding("text/html;charset=utf-8");
		request.setCharacterEncoding("UTF-8");
		
		/*
		1. 獲取參數 username
		2. 判斷是否為 jack870131
	    3. 如果是: 響應1
		4. 如果不是: 響應0
		*/
		String username = request.getParameter("username");
		if(username.equalsIgnoreCase("jack870131")) {
			response.getWriter().print("1");
		} else {
			response.getWriter().print("0");
		}
	}
}

//ajax3.jsp
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

<title>My JSP 'ajax3.jsp' starting page</title>

<meta http-equiv="pragma" content="no-cache">
<meta http-equiv="cache-control" content="no-cache">
<meta http-equiv="expires" content="0">
<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
<meta http-equiv="description" content="This is my page">
<!--
	<link rel="stylesheet" type="text/css" href="styles.css">
	-->
<script type="text/javascript">
	//創建異步對象
	function createXMLHttpRequest() {
		try {
			return new XMLHttpRequest();
		} catch (e) {
			try {
				return ActiveXObject("Msxml2.XMLHTTP");
			} catch (e) {
				try {
					return ActiveXObject("Microsoft.XMLHTTP");
				} catch (e) {
					alert("What web explorer you are using?");
					throw e;
				}
			}
		}
	}
	
	window.onload = function() {
		//獲取文本框，給他的失去焦點事件註冊監聽
		var userEle = document.getElementById("usernameEle");
		userEle.onblur = function() {
			//1. 得到異步對象
			var xmlHttp = createXMLHttpRequest();
			//2. 打開鏈接
			xmlHttp.open("POST", "<c:url value='/ValidateUsernameServlet'/>", true);
			//3. 設置請求頭
			xmlHttp.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
			//4. 發送請求，給出請求體
			xmlHttp.send("username=" + userEle.value);
			//5. 給 xmlHttp 的 onreadystatechange 事件註冊監聽
			xmlHttp.onreadystatechange = function() {
				if(xmlHttp.readyState == 4 && xmlHttp.status == 200) { //雙重判斷
					//獲取服務器的響應，判斷是否為1
					//是: 獲取 span，添加內容: "用戶名已被註冊"
					var text = xmlHttp.responseText;
					var span = document.getElementById("errorSpan");
					if(text == "1") {
						//得到 span 元素
						span.innerHTML = "用戶名已被註冊";
					} else {
						span.innerHTML = "";							
					}
				}
			};
		};
	};
</script>
</head>

<body>
	<h1>演示用戶名是否已被註冊</h1>
	<form action="" method="post">
		用戶名: <input type="text" name="username" id="usernameEle"><span
			id="errorSpan"></span><br> 密 碼: <input type="password"
			name="password"><br> <input type="submit" name="註冊">
	</form>
</body>
</html>
```


### 響應內容為 xml
- 服務器端:
    - 設置響應頭: ContentType, 其值為: text/xml;charset=utf-8
- 客戶端:
    - var doc = xmlHttp.responseXML; //得到的是 Document 對象
    
```java
//web.xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>Java_Web</display-name>
  <servlet>
    <servlet-name>BServlet</servlet-name>
    <servlet-class>pers.servlet.BServlet</servlet-class>
  </servlet>

  <servlet-mapping>
    <servlet-name>BServlet</servlet-name>
    <url-pattern>/BServlet</url-pattern>
  </servlet-mapping>
  <welcome-file-list>
    <welcome-file>index.jsp</welcome-file>
  </welcome-file-list>
</web-app>

//BServlet.java
package pers.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class BServlet extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html");
		String xml = "<students>" + 
				"<student number='JACK_1001'>" + 
				"<name>Jack</name>" + 
				"<age>20</age>" + 
				"<sex>male</sex>" + 
				"</student>" + 
				"</students>";
		
		response.setContentType("text/xml;charset=utf-8");
		response.getWriter().print(xml);
	}
}

//ajax4.jsp
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

<title>My JSP 'ajax4.jsp' starting page</title>

<meta http-equiv="pragma" content="no-cache">
<meta http-equiv="cache-control" content="no-cache">
<meta http-equiv="expires" content="0">
<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
<meta http-equiv="description" content="This is my page">
<!--
	<link rel="stylesheet" type="text/css" href="styles.css">
	-->
<script type="text/javascript">
	//創建異步對象
	function createXMLHttpRequest() {
		try {
			return new XMLHttpRequest();
		} catch (e) {
			try {
				return ActiveXObject("Msxml2.XMLHTTP");
			} catch (e) {
				try {
					return ActiveXObject("Microsoft.XMLHTTP");
				} catch (e) {
					alert("What web explorer you are using?");
					throw e;
				}
			}
		}
	}

	window.onload = function() { //文本加載完畢後執行
		var btn = document.getElementById("btn");
		btn.onclick = function() { //給按鈕的點擊事件註冊監聽
			/*
			ajax 四步操作，得到服務器的響應
			把響應結果顯示到 h1 元素中
			*/
			/*
			1. 得到異步對象
			*/
			var xmlHttp = createXMLHttpRequest();
			/*
			 2. 打開與服務器的連接
			  - 指定請求方式
			  - 指定請求的 URL
			  - 指定是否為異步請求
			*/
			//修改 open 方法，指定請求方式為 POST
			xmlHttp.open("GET", "<c:url value='/BServlet'/>", true);
			/*
			 3. 發送請求
			*/
			xmlHttp.send(null); //GET 請求沒有請求體，但也要給出 null, 不然 FireFox 可能會不能發送
			/*
			 4. 給異步對象的 onreadystatechange 事件註冊監聽器
			*/
			xmlHttp.onreadystatechange = function() { //當 xmlHttp 的狀態發生變化時執行
				//雙重判斷: xmlHttp 的狀態為4(服務器響應結束), 以及服務器響應的狀態碼為200(響應成功)
				if (xmlHttp.readyState == 4 && xmlHttp.status == 200) {
					//獲取服務器的響應結果(xml)
					var doc = xmlHttp.responseXML;
					//查詢文檔下名為 student 的所有元素，得到數組，再取下標0元素
					var ele = doc.getElementsByTagName("student")[0];
					var number = ele.getAttribute("number"); //获取元素名为number的属性值
					var name;
					var age;
					var sex;

					// 处理浏览器的差异
					if (window.addEventListener) {
						name = ele.getElementsByTagName("name")[0].textContent; //其他浏览器
					} else {
						name = ele.getElementsByTagName("name")[0].text; //IE支持
					}
					if (window.addEventListener) {
						age = ele.getElementsByTagName("age")[0].textContent; //其他浏览器
					} else {
						age = ele.getElementsByTagName("age")[0].text; //IE支持
					}
					if (window.addEventListener) {
						sex = ele.getElementsByTagName("sex")[0].textContent; //其他浏览器
					} else {
						sex = ele.getElementsByTagName("sex")[0].text; //IE支持
					}

					var text = number + ", " + name + ", " + age + ", " + sex;
					document.getElementById("h1").innerHTML = text;
				}
			};
		};
	};
</script>
</head>

<body>
	<button id="btn">點擊這裡</button>
	<h1 id="h1"></h1>
</body>
</html>
```


### 省市聯動
- 頁面
    ```xml
    <select name="province">
        <option>===請選擇省分===</option>
    </select>
    <select name="city">
        <option>===請選擇城市===</option>
    </select>
    ```
    
- Servlet
    - ProvinceServlet: 當頁面加載完畢後馬上請求這個 Servlet
        - 他需要加載 China.xml 文件，把所有的省名稱使用字符串發送給客戶端
        
- 頁面的工作
    - 獲取這個字符串，使用逗號分隔，得到數組
    - 循環遍歷每個字符串(省分的名稱)，使用每個字符串創建一個 &lt;option&gt; 元素添加到 &lt;select name="province"&gt; 這個元素中

- CityServlet
    - CityServlet: 當頁面選擇某個省時，發送請求。
    - 得到省分的名稱: 加載 china.xml 文件，查詢出該省分對應的元素對象，把這個元素換成 xml 字符串，發送給客戶端。
    
- 頁面的工作
    - 把 &lt;select name="city"&gt; 中的所有子元素刪除，但不要刪除<option>===請選擇城市===</option>
    - 得到服務器的響應結果: doc。
    - 獲取所有的 &lt;city&gt; 子元素，循環遍歷，得到&lt;city&gt; 的內容
    - 使用每個 &lt;city&gt; 的內容創建一個&lt;option&gt; 元素，添加到 &lt;select name="city"&gt;
    
![](https://github.com/jack870131/Markdown-Pic/blob/master/Picture/ajax2.png?raw=true)

```java
//web.xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>Java_Web</display-name>
  <servlet>
    <servlet-name>ProvinceServlet</servlet-name>
    <servlet-class>pers.servlet.ProvinceServlet</servlet-class>
  </servlet>
  <servlet>
    <servlet-name>CityServlet</servlet-name>
    <servlet-class>pers.servlet.CityServlet</servlet-class>
  </servlet>

  
  <servlet-mapping>
    <servlet-name>ProvinceServlet</servlet-name>
    <url-pattern>/ProvinceServlet</url-pattern>
  </servlet-mapping>
  <servlet-mapping>
    <servlet-name>CityServlet</servlet-name>
    <url-pattern>/CityServlet</url-pattern>
  </servlet-mapping>
  <welcome-file-list>
    <welcome-file>index.jsp</welcome-file>
  </welcome-file-list>
</web-app>

//ProvinceServlet.java
package pers.servlet;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.dom4j.Attribute;
import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.io.SAXReader;

public class ProvinceServlet extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		/*
		 * 響應所有省分名稱，使用逗號分隔
		 */
		/*
		 * 1. Document 對象 - 創建解析器對象 - 調用解析器的讀方法，傳遞一個流對象，得到Document
		 */
		try {
			SAXReader reader = new SAXReader();
			InputStream input = this.getClass().getResourceAsStream("/china.xml");
			Document doc = reader.read(input);
			/*
			 * 查詢所有 province 的 name 屬性，得到一堆屬性對象
			 * 循環遍歷，把所有的屬性值連接成一個字符串，發送給客戶端
			 */
			List<Attribute> arrList = doc.selectNodes("//province/@name");
			StringBuilder sb = new StringBuilder();
			for(int i = 0; i < arrList.size(); i++) {
				sb.append(arrList.get(i).getValue()); //把每個屬性的值存放到 sb 中
				if(i < arrList.size() - 1) {
					sb.append(",");
				}
			}
			response.getWriter().print(sb);
		} catch (DocumentException e) {
			throw new RuntimeException(e);
		}
	}
}

//CityServlet.java
    package pers.servlet;

import java.io.IOException;
import java.io.InputStream;
import java.io.PrintWriter;
import java.util.List;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.dom4j.Attribute;
import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

public class CityServlet extends HttpServlet {
	public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/xml;charset=utf-8"); //注意: 發送 xml 時這裡要修改
		
		/*
		 * 獲取省分名稱，加載該省對應的 <province> 元素
		 * 把元素轉換成字符串發送到客戶端
		 */
		/*
		 * 1. 獲取省分名稱
		 * 2. 使用省分名稱查找到對應個<province>元素
		 * 3. 把<province>元素轉換成字符串，發送
		 */
		try {
			/*
			 * 得到 Document
			 */
			SAXReader reader = new SAXReader();
			InputStream input = this.getClass().getResourceAsStream("/china.xml");
			Document doc = reader.read(input);
			
			/*
			 * 獲取參數
			 */
			String pname = request.getParameter("pname"); //獲取省分名稱
			Element proEle = (Element) doc.selectSingleNode("//province[@name='" + pname + "']"); //province[@name='北京']
			String xmlStr = proEle.asXML(); //把元素轉換成字符串
			response.getWriter().print(xmlStr);
		} catch (DocumentException e) {
			throw new RuntimeException(e);
		}
	}
}

//ajax5.jsp
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

<title>My JSP 'ajax5.jsp' starting page</title>

<meta http-equiv="pragma" content="no-cache">
<meta http-equiv="cache-control" content="no-cache">
<meta http-equiv="expires" content="0">
<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
<meta http-equiv="description" content="This is my page">
<!--
	<link rel="stylesheet" type="text/css" href="styles.css">
	-->
<script type="text/javascript">
	//創建異步對象
	function createXMLHttpRequest() {
		try {
			return new XMLHttpRequest();
		} catch (e) {
			try {
				return ActiveXObject("Msxml2.XMLHTTP");
			} catch (e) {
				try {
					return ActiveXObject("Microsoft.XMLHTTP");
				} catch (e) {
					alert("What web explorer you are using?");
					throw e;
				}
			}
		}
	}
	/*
	 * 1. 在文檔加載完畢時，得到所有省分名稱，顯示在<select name="province"/>中
	 * 2. 在選擇了新的省分時，發送請求(參數為省名稱)，得到 xml 文檔，及<province>元素
	 *    解析xml文檔，得到其中所有的<city>，再得到每個<city>元素的內容，及市名，使用市名生成<option>，差如到<select>元素中
	 */
	window.onload = function() {
		/*
		 * ajax 四步，請求 ProvinceServlet, 得到所有省分名稱
		 * 使用每個省份名稱創建一個<option>元素，添加到<servlet name="province">中	
		 */
		var xmlHttp = createXMLHttpRequest();
		xmlHttp.open("GET", "<c:url value='/ProvinceServlet'/>", true);
		xmlHttp.send(null);
		xmlHttp.onreadystatechange = function() {
			if (xmlHttp.readyState == 4 && xmlHttp.status == 200) {
				//獲取服務器的響應
				var text = xmlHttp.responseText;
				//使用逗號分隔，得到數組
				var arr = text.split(",");
				//循環遍歷每個省份名稱，每個名稱生成一個option對象，添加到<servlet>中
				for (var i = 0; i < arr.length; i++) {
					var op = document.createElement("option"); //創建一個指定名稱元素
					op.value = arr[i]; //設置 op 的實際值為當前的省分
					var textNode = document.createTextNode(arr[i]); //創建文本節點
					op.appendChild(textNode); //把文本子節點添加到op元素中，指定其顯示值

					document.getElementById("p").appendChild(op);
				}
			}
		};

		/*
		給<select name="province"> 添加改變監聽
		使用選擇的省份名稱請求CityServlet, 得到<province>元素(xml元素)
		獲取<province>元素中所有的<city>元素，遍歷。獲取每個<city>的文本內容，即市名稱
		使用每個市名稱創建<option>元素添加到<select name="city">
		*/
		var proSelect = document.getElementById("p");
		proSelect.onchange = function() {
			var xmlHttp = createXMLHttpRequest();
			xmlHttp.open("POST", "<c:url value='/CityServlet'/>", true);
			xmlHttp.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
			xmlHttp.send("pname=" + proSelect.value); //把下拉列表中選擇的值發送給服務器
			xmlHttp.onreadystatechange = function() {
				if (xmlHttp.readyState == 4 && xmlHttp.status == 200) {
					/*
					把 select 中所有的 option 移除(除了選擇)
					*/
					var citySelect = document.getElementById("c");
					//獲取其所有子元素
					var optionEleList = citySelect.getElementsByTagName("option");
					//循環遍歷每個 option 元素，然後再 citySelect 中移除
					while (optionEleList.length > 1) { //子元素的個數如果大於1就循環，等於1就不循環
						citySelect.removeChild(optionEleList[1]); //總是刪除1下標，2就變成1了
					}

					var doc = xmlHttp.responseXML;
					//得到所有名為 city 的元素
					var cityEleList = doc.getElementsByTagName("city"); //得到每個 city 元素
					//循環遍歷每個 city 元素
					for (var i = 0; i < cityEleList.length; i++) {
						var cityEle = cityEleList[i]; //支持 FireFox 等瀏覽器
						var cityName;
						//獲取市名稱
						if (window.addEventListener) { //處理瀏覽器的差異
							cityName = cityEle.textContent; //支持 FireFox 等瀏覽器
						} else {
							cityName = cityEle.text; //支持 IE
						}

						//使用市名稱創建 option 元素，添加到 <select name="city"> 中
						var op = document.createElement("option");
						op.value = cityName;
						//創建文本節點
						var textNode = document.createTextNode(cityName);
						op.appendChild(textNode); //把文本節點追加到 op 元素中

						//把 op 添加到 <select> 元素中
						var citySelect = document.getElementById("c");
						citySelect.appendChild(op);
					}
				}
			};
		};
	};
</script>
</head>

<body>
	<h1>省市聯動</h1>
	<select name="province" id="p">
		<option>===請選擇省分===</option>
	</select>
	<select name="city" id="c">
		<option>===請選擇城市===</option>
	</select>
</body>
</html>
```
