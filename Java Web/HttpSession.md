# HttpSession

## 概述
- HttpSession 是由 JavaWeb 提供的，用來會話跟蹤的類。Session 是**服務器端對象**，保存在服務器端。
- HttpSession 是 Servlet 三大域對象 (request, session, application (ServletContext)) 之一，所以它也有 setAttribute(), getAttribute(), removeAttribute() 方法。
- HttpSession 底層依賴 Cookie, 或是 URL 重寫。

## 作用
- 會話範圍: 會話範圍是某個用戶從首次訪問服務器開始，到該用戶關閉瀏覽器結束。
 - 會話: 一個用戶對服務器的多次連貫性請求。所謂連關性請求，就是該用戶多次請求中間沒有關閉瀏覽器。
- 服務器會為每個客戶端創建一個 Session 對象，Session 就好比客戶在服務器端的帳戶，他們被服務器保存到一個 Map 中，這個 **Map** 被稱為 **Session 緩存**。
 - Servlet 中得到 Session 對象: HttpSession session = request.getSession();
 - JSP 中得到 Session 對象: Session 是 JSP 內置對象之下，不用創建就可以直接使用。
- Session 域相關方法:
 - void setAttribute(String name, Object value);
 - Object getAttribute(String name);
 - void removeAttribute(String name);

## 演示 Session 中會話的多次請求中共享數據
- AServlet: 向 Session 域中保存數據。
- BServlet: 向 Session 域中獲取數據。
- 演示
 - 第一個請求: 訪問 AServlet
 - 第二個請求: 訪問 BServlet
```JSP
<!-- a.jsp -->
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
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
  	<h1>保存 Cookie</h1>
  	<%-- request, response, session, application, pageContext, config, out, page, exception --%>
  	<%
  		Cookie cookie1 = new Cookie("aaa", "AAA");
  		response.addCookie(cookie1);
  		Cookie cookie2 = new Cookie("bbb", "BBB");
  		response.addCookie(cookie2);
  	%>
  </body>
</html>

<!-- b.jsp -->
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
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
  	<h1>獲取 Cookie</h1>
  	<%
  		Cookie[] cookies = request.getCookies();
  		if(cookies != null) {
  			for(Cookie c: cookies) {
  				out.print(c.getName() + "=" + c.getValue() + "<br>");
  			}
  		}
  	%>
  </body>
</html>
```

## 演示保存用戶登錄信息
- 案例相關頁面和 Servlet:
 - login.jsp: 登錄頁面
 - succ1.jsp: 只有登錄成功才能訪問的頁面。
 - succ2.jsp: 只有登錄成功才能訪問的頁面。
 - LoginServlet: 校驗用戶是否登錄成功。
- 各頁面和 Servlet 內容:
 - login.jsp: 題更登錄表單，提交表單請求 LoginServlet
 - LoginServlet: 獲取請求參數，校驗用戶是否登錄成功。
        - 失敗: 保存錯誤信息到 request 域，轉發到 login.jsp (login.jsp 顯示 request 域中的錯誤信息)
        - 成功: 保存用戶信息到 Session 域中，重定向到 succ1.jsp (login.jsp 顯示 request 域中的用戶信息)
 - succ1.jsp: 從 Session 域獲取用戶信息，如果不存在，顯示"您還沒有登錄"。存在則顯示用戶信息。
 - succ2.jsp: 從 Session 域獲取用戶信息，如果不存在，顯示"您還沒有登錄"。存在則顯示用戶信息。 
- **注意:** 只要用戶沒有關閉瀏覽器，Session 就一直存在，那麼保存在 Session 中的用戶信息也就一起存在，那麼用戶訪問 succ1 & succ2 就會通過。
```JSP
<!-- login.jsp -->
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
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
  	<%-- 本頁面提供登錄表單，還要顯示錯誤信息 --%>
  	<h1>登錄</h1>
  	<%
  		/*
  		 * 讀取名為 uname 的 Cookie
  		 * 如果為空顯示: ""
  		 * 如果不為空顯示: Cookie 的值
  		 */
  		String uname = "";
  		Cookie[] cs = request.getCookies();
  		if(cs != null) { //如果存在 Cookie
  			for(Cookie c : cs) { //循環遍歷所有 Cookie
  				if("uname".equals(c.getName())) { //查找名為 uname 的 Cookie
  					uname = c.getValue(); //獲取這個 Cookie 的值，給 uname 這個變量
  				}
  			}
  		}
  	%>
  	<%
  		String message = "";
  		String msg = (String)request.getAttribute("msg"); //獲取 request 域中名為 msg 的屬性
  		if(msg != null) {
  			message = msg;
  		}
  	%>
  	<font color="red"><b><%=message %></b></font>
  	<form action="/Java_Web/LoginServlet" method="post">
  				<%-- 把 Cookie 中的用戶名顯示到用戶名文本框中 --%>
  		用戶名: <input type="text" name="username" value="<%=uname%>"><br>
  		密   碼: <input type="password" name="password"><br>
  		<input type="submit" value="登錄">
  	</form>
  </body>
</html>

<!-- succ1.jsp -->
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <base href="<%=basePath%>">
    
    <title>My JSP 'succ1.jsp' starting page</title>
    
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
  	<h1>succ1</h1>
  	<%
  		String username = (String)session.getAttribute("username");
  		if(username == null) {
  			/*
  			 * 1. 向 request 域中保存錯誤信息，轉發到 login.jsp
  			 */
  			request.setAttribute("msg", "您還未登錄!");
  			request.getRequestDispatcher("/session2/login.jsp").forward(request, response);
  			return;
  		}
  	%>
  	歡迎<%=username %>
  </body>
</html>

<!-- succ2.jsp -->
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <base href="<%=basePath%>">
    
    <title>My JSP 'succ2.jsp' starting page</title>
    
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
    <h1>succ2</h1>
  	<%
  		String username = (String)session.getAttribute("username");
  		if(username == null) {
  			/*
  			 * 1. 向 request 域中保存錯誤信息，轉發到 login.jsp
  			 */
  			request.setAttribute("msg", "您還未登錄!");
  			request.getRequestDispatcher("/session2/login.jsp").forward(request, response);
  			return;
  		}
  	%>
  	歡迎<%=username %>
  </body>
</html>

<!-- LoginServlet.java -->
package servlet_8;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

public class LoginServlet extends HttpServlet {

	public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		/*
		 * 1. 獲取表單數據
		 */
		//處理中文問題
		request.setCharacterEncoding("utf-8");
		//獲取
		String username = request.getParameter("username");
		String password = request.getParameter("password");
		/*
		 * 2. 校驗用戶名和密碼是否正確
		 */
		if(!"jack870131".equalsIgnoreCase(username)) { //登錄成功
			/*
			 * 附加項: 把用戶名保存到 Cookie 中，發送給客戶端瀏覽器
			 * 當再次打開 login.jsp 時，login.jsp 中會讀取 request 中的 Cookie, 把它顯示到用戶名文本框中
			 */
			Cookie cookie = new Cookie("uname", username); //創建 Cookie
			cookie.setMaxAge(60 * 60 * 24); //設置 Cookie 命長為一天
			response.addCookie(cookie); //保存 Cookie
			
			/*
			 * 3. 如果成功
			 *  - 保存用戶信息到 Session 中
			 *  - 重定向到 succ1.jsp
			 */
			HttpSession session = request.getSession();//獲取 Session
			session.setAttribute("username", username);
			response.sendRedirect("/Java_Web/session2/succ1.jsp");
		} else { //登錄失敗
			/*
			 * 4. 如果失敗
			 *  - 保存錯誤信息到 request 域中
			 *  - 轉發到 login.jsp
			 */
			request.setAttribute("msg", "用戶名或密碼錯誤");
			RequestDispatcher rd = request.getRequestDispatcher("/session2/login.jsp"); //得到轉發器
			rd.forward(request, response); //轉發
		}
	}
}
```


## 原理
- request.getSession() 方法
 - 獲取 Cookie 中的 JSESSIONID
      - 如果 JSESSIONID 不存在，創建 Session, 把 Session 保存起來，把新創建的 JSESSIONID 保存到 Cookie 中。
      - 如果 JSESSIONID 存在，通過 JSESSIONID 查找 Session 對象，如果沒有查找到，創建 Session, 把 Session 保存起來，把新創建的 JSESSIONID 保存到 Cookie 中。
      - 如果 JSESSIONID 存在，通過 JSESSIONID 查找到了 Session 對象，那麼就不會再創建 Session 對象了。
      - 返回 Session
 - 如果創建了新的 Session, 瀏覽器會得到一個包含了 JSESSIONID 的 Cookie, 這個 Cookie 的生命為 -1, 即只在瀏覽器內存中存在，如果不關閉瀏覽器，那麼 Cookie 就不會消失。
 - 下次請求時，再次執行 request.getSession() 方法時，因為可以通過 Cookie 中的 JSESSIONID 找到 Session 對象，所以上一次請求使用的是同一 Session 對象。
- 服務器不會馬上給你創建 Session, 在**第一次獲取 Session** 時，才會創建 -> request.getSession();
- request.getSession(false), request.getSession(true), request.getSession(), 後兩方法效果相同。
  第一個方法: 如果 Session 緩存中(如果 Cookie 不存在)不存在 Session, 那麼返回 null, 而不會創建 Session 對象。


## 其他方法
- String getId(): 獲取 JSESSIONID
- int getMaxINactiveInterval(): 獲取 Session 可以的最大部活動時間(秒)，默認為 30 分鐘，當 Session 在 30 分鐘內沒有使用，那麼 Tomcat 會在 Session 池中移除。
- void invalidate(): 讓 Session 失效，調用這方法會讓 Session 失效，當 Session 失效後，客戶端再次請求，服務器會給客戶端創建一個 Session, 並在響應中給客戶端新 Session 的 JSESSIONID
- boolean isNew(): 查看 Session 是否為新。當客戶端第一次請求時，服務器為客戶端創建 Session, 但這時服務器還沒有響應客戶端，也就是還沒有把 JSESSIONID 響應給客戶端時，這時 Session 的狀態為新。

## web.xml 中配置 Session 的最大部活動時間
```xml
<session-config>
    <session-timeout>30</session-timeout>
</session-config>
```

## URL 重寫
- 就是把所有頁面中的路徑，都使用 response.encodeURL("...") 處理一下。
- Session 依賴 Cookie, 目的是讓客戶端發出請求時歸還 JSESSIONID, 這樣才能找到對應的 Session
- 如果客戶端禁用了 Cookie, 那麼就無法得到 JSESSIONID, 那麼 session 也就無用。
- 也可以使用 URL 重寫來提替代 Cookie
 - 讓網站的所有超鏈接, 表單中都添加一個特殊的請求參數，即 JSESSIONID
 - 這樣服務器可以通過獲取請求參數得到 JSESSIONID, 從而找到 session 對象。
- response.encodeURL(String url)
 - 該方法會對 URL 進行智能的重寫: 當請求中沒有歸還 JSESSIONID 這個 Cookie, 那麼該方法會重寫 URL, 否則不重寫。當然 URL 必須是指向本站的 URL。

```JSP
<!-- a.jsp -->
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
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
  	<a href="/Java_Web/AServlet;JSESSIONID=<%=session.getId()%>">Click Here</a>
  	<a href="/Java_Web/AServlet;JSESSIONID=<%=session.getId()%>">Click Here</a>
  	<a href="/Java_Web/AServlet;JSESSIONID=<%=session.getId()%>">Click Here</a>
  	
  	<%
  		/*
  		 * 查看 Cookie 是否存在，如果不存在，在指定的 URL 之後添加 JSESSIONID 參數
  		 * 如果存在，它就不會在 URL 之後添加任何東西
  		 */
  		out.println(response.encodeURL("/Java_Web/AServlet"));
  	%>
  </body>
</html>

<!--AServlet.java -->
public class AServlet extends HttpServlet {

	public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		request.getSession();
		response.getWriter().print("請查看使否有收到JSESSIONID這個Cookie");
	}
}
```
