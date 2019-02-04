# Cookie

## Http & Cookie
- Cookie 是 Http 協議制定的。先由服務器保存 Cookie 到瀏覽器，再下次瀏覽器請求服務器時把上一次請求得到 Cookie 再歸還給服務器。
- 由服務器創建保存到客戶端瀏覽器的一個鍵值對。服務器保存 Cookie 的響應頭: Set-Cookie: aaa=AAA Set-Cookie: bbb=BBB
- 當瀏覽器請求服務器時，會把該服務器保存的 Cookie 隨請求發送給服務器。瀏覽器歸還 Cookie 的請求頭: Cookie: aaa=AAA; bbb=BBB
- Http 協議規定 (保證不給瀏覽器太大壓力):
 - 一個 Cookie 最大 4KB
 - 一個服務器最多向一個瀏覽器保存 20 個 Cookie
 - 一個瀏覽器最多可以保存 300 個 Cookie
- 瀏覽器大戰: 因為瀏覽器競爭很激烈，所以很多瀏覽器會再一定範圍內違反 Http 規定，但也不會讓一個 Cookie 過大。

## 用途
- 服務器使用 Cookie 來跟蹤客戶端狀態。
- 保存購物車(購物車中的商品不能使用 request 請求，因為它是一個用戶向服務器發送的多個請求信息)
- 顯示上交登錄名(也是一個用戶多個請求)
- **重點:** Cookie 是**不能跨瀏覽器**的。

## JavaWeb 中使用 Cookie
- 原始方式
 - 使用 **response** 發送 Set-Cookie 響應頭
 - 使用 **request** 獲取 Cookie 請求頭
- 便捷方式
 - 使用 response.addCookie() 方法向瀏覽器保存 Cookie
 - 使用 request.getCookies() 方法獲取瀏覽器歸還的 Cookie
- Example
 - 一個 jsp 保存 Cookie，a.jsp
 - 另一個 jsp 獲取瀏覽器歸還的 Cookie，b.jsp
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
        

## Cookie 詳解
- Cookie 不只有 name 和 value 兩個屬性。
- Cookie 的 maxAge: Cookie 的最大生命，即 Cookie 可保存的**最大時長**，以秒為單位。例如: cookie.setMaxAge(60) 表示這個 Cookie 會被瀏覽器保存到硬盤。
 - maxAge > 0: 瀏覽器會把 Cookie 保存到客戶機硬盤上，有效時常為 maxAge 的值決定。
 - maxAge < 0: Cookie 只在瀏覽器內存中存在，當用戶關閉瀏覽器時，瀏覽器進程結束，同時 Cookie 也就死亡了。
 - maxAge = 0: 瀏覽器會馬上刪除這個 Cookie。
    ```
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
      	<h1>顯示 Cookie 的 MaxAge</h1>
      	<%
      		Cookie cookie1 = new Cookie("aaa", "AAA");
      		cookie1.setMaxAge(60 * 60);
      		response.addCookie(cookie1);
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
      	<h1>刪除 Cookie</h1>
      	<%
      		Cookie cookie1 = new Cookie("aaa", "AAA");
      		cookie1.setMaxAge(0);
      		response.addCookie(cookie1);
      	%>
      </body>
    </html>
    ```
 

## Cookie 的 path:
- Cookie 的 path 並不是設置這個 Cookie 再客戶端的保存路徑。
- Cookie 的 path 由服務器創建 Cookie 時設置。
- 當瀏覽器訪問服務器某個路徑時，由 Cookie 的 path 決定需要歸還那些 Cookie 給服務器。
- 瀏覽器訪文服務器的路徑，如果包含某個 Cookie 的路徑，哪麼就會**歸還**這個 Cookie。
- Example
 - aCookie.path = /Java_Web/; bCookie.path = /Java_Web/JSP/; cCookie.path = /Java_Web/JSP/Cookie/;
 - 訪問: /Java_Web/index.jsp 時，歸還: aCookie
 - 訪問: /Java_Web/JSP/a.jsp 時，歸還: aCookie, bCookie
 - 訪問: /Java_Web//JSP/Cookie/b.jsp 時，歸還: aCookie, bCookie, cCookie
- Cookie 的 path 默認值: 當前訪問路徑的父路徑。例如在訪問 /Java_Web/JSP/a.jsp 時，響應的 Cookie, 那麼這個 Cookie 的默認 path 為 /Java_Web/JSP/

## Cookie 的 domain (域)
- domain 用來指定 Cookie 的域名，當多個二級域中共享 Cookie 時才有用。
- 例如: www.baidu.com, tieba.baidu.com, news.baidu.com 之間共享 Cookie 時可以使用 domain
- 設置 domain 為: cookie.setDomain(".baidu.com");
- 設置 path 為: cookie.setPath("/");
