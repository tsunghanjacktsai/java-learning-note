# JSP

## 定義
JSP(Java Server Pages) 是 JavaWeb 服務器端的動態資源。它與 html 頁面的作用是相同的，**顯示數據**和**獲取數據**。

## 作用
- Servlet
 - 缺點: 不是和設置 html 響應體，需要大量的 response.getWriter().print("html")
 - 優點: 動態資源，可以編程。
- HTML
 - 缺點: html 是靜態頁面，不能包含動態信息。
 - 優點: 不用為輸出 html 而發愁。
- JSP
 - 優點: 在原有 html 基礎上添加 Java 腳本，構成 jsp 頁面。

## JSP & Servlet 分工
- JSP
 - 作為請求發起頁面，例如顯示表單、超鏈接。
 - 作為請求結束頁面，例如顯示數據。
- Servlet
 - 作為請求中處理數據的環節。

 ![](https://github.com/jack870131/Markdown-Pic/blob/master/Picture/JSP.png)
 

## JSP 組成
- jsp = html + java 腳本 + jsp 標籤(指令)
- jsp 中無須創建一可使用的對象一共有 9 個，被稱之為 9 大內置對象，例如: request 對象、out 對象。
- 3 種 Java 腳本
1. %...%: **Java 代碼片段**(常用)，用於定義 0 ~ n 條 Java 語句。-> 方法內能寫什麼，它就可以放甚麼。
2. %=...%: **Java 表達式**，用於輸出(常用)，用於輸出 條表達式(或變量)的結果。-> response.getWriter().print(...)，這裡能放甚麼，它就可以放甚麼
3. %!...%: **聲明**，用來創建類的成員變量和成員方法(基本不用)。-> 成員、方法、構造器、構造代碼塊、靜態塊、內部類
- Example
 - 演示 jsp 中 java 腳本的使用
    ```
    <%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
    <%
    	String path = request.getContextPath();
    	String basePath = request.getScheme() + "://" + request.getServerName() + ":" + request.getServerPort()
    			+ path + "/";
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
    	<table border="2px black solid" align="center" width="80%">
    		<tr>
    			<td>Name</td>
    			<td>Age</td>
    		</tr>
    		<%
    			for(int i = 0; i < 10; i++) {
    		%>
    		<tr>
    			<td>Jack</td>
    			<td>20</td>
    		</tr>
    		<%
    			}
    		%>
    	</table>
    </body>
    </html>
    ```

 - 演示 jsp & Servlet 分工
 ![](leanote://file/getImage?fileId=5ae1c5dc50b7c71a3c000000)

    ```html
    <!-- form.jsp -->
    <%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
    <%
    String path = request.getContextPath();
    String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
    %>
    
    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
    <html>
      <head>
        <base href="<%=basePath%>">
        
        <title>My JSP 'form.jsp' starting page</title>
        
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
      	<form action="/Java_Web/ServletDemo" method="post">
      		整數1: <input type="text" name="num1"><br>
      		整數2: <input type="text" name="num2"><br>
      		<input type="submit" value="提交">
      	</form>
      </body>
    </html>
    
    <!-- result.jsp -->
    <%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
    <%
    String path = request.getContextPath();
    String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
    %>
    
    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
    <html>
      <head>
        <base href="<%=basePath%>">
        
        <title>My JSP 'result.jsp' starting page</title>
        
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
    	<%
    		Integer result = (Integer) request.getAttribute("result");
    	%>
    	<%=
    		result 	
    	%>
      </body>
    </html>
    
    //ServletDemo.java
    import java.io.IOException;
    import java.io.PrintWriter;
    
    import javax.servlet.ServletException;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    
    public class ServletDemo extends HttpServlet {
    
    	public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    		//獲取參數
    		String s1 = request.getParameter("num1");
    		String s2 = request.getParameter("num2");
    		//轉換成 int 類型
    		int num1 = Integer.parseInt(s1);
    		int num2 = Integer.parseInt(s2);
    		//運算
    		int sum = num1 + num2;
    		//把結果保存到 request 域中
    		request.setAttribute("result",sum);
    		//轉換到 result.jsp
    		request.getRequestDispatcher("/Demo1/result.jsp").forward(request, response);;
    	}
    }
    ```

## JSP 原理
- jsp 其實是一種特殊的 Servlet
 - 當 jsp 頁面第一次被訪問時，服務器會把 jsp 編譯成 java 文件 (這個 java 其實是一個 Servlet 類)
 - 然後把 java 編譯成 .class
 - 然後創建該類對象
 - 最後調用它的 service() 方法
 - 第二次請求同一 jsp 時，直接調用 service() 方法
- 在 Tomcat 的 work 目錄下可以找到 jsp 對應的 java 源碼。
- 查看 jsp 對應 java 文件
 - java 腳本
 - html

## JSP 註釋
- <%--...--%>: 當服務器把 jsp 編譯成 java 文件時已經忽略註釋部分。

## JSP 三大指令
page 指令是最為常用的指定，也是屬性最多的指令。
page 指令沒有必須屬性，都是可選屬型。沒有給出任何屬性也是可以的。
在 jsp 也面中，任何指令都可以重複出現。

- page -> 最複雜: &lt; %@page language = "java" info="xxx"...% &gt;
 - pageEncoding & contentType: pageEncoding 它指定當前jsp頁面的編碼，只要不說謊，就不會有亂碼。在服務器要把 jsp 編譯成 .jave 時需要使用 pageEncoding。
     - contentType: 表示添加一個響應頭 -- Content-Type。等同於 response.setContentType("text/html;charset=utf-8");
     - 如果兩個屬性只提供一個，另一個的默認值為設置那一個。
     - 如果兩個屬性都沒有設置，那麼默認為iso。
 - import: 導包，可以出現多次。
 - errorPage 和 isErrorPage
     - errorPage: 當前頁面如果拋出異常，那麼要轉發到哪一個頁面，由 errorPage 來指定。
     - isErrorPage: 它指定當前頁面。這個頁面會設置狀態碼為500。而且這個頁面可以使用9大內置對象中的 exception。

        ```html
        	<error-page>
        		<error-code>404</error-code>
        		<location>/error/errorPage.jsp</location>
        	</error-page>
        ```
 - autoFlush & buffer
    - autoFlush: 指定 jsp 的輸出流緩衝區滿時，是否自動刷新。默認為 true，如果為false，那麼在緩衝區滿時拋出異常。
    - buffer: 指定緩衝區大小，默認為 8kb，通常不需要修改。
 - isELIgnored: 是否忽略 el 表達式，默認值為false，不忽略，即支持。
 - language: 指定當前 jsp 編譯後的語言類型，默認值為java。
 - info: 信息。
 - isThreadSafe: 當前的 jsp 是否支持併發訪問。
 - session: 當前頁面是否支持 session，如果為 false，那麼當前頁面就沒有 session 這個內置對象。
 - extends: 讓 jsp 生成的 servlet 去繼承該屬性指定的類。
- include -> 靜態包含
    - 與 RequestDispatcher 的 include() 方法的功能相似。
    - <%@include%> 他是在編譯成 java 文件時完成的。他們共同生成一個 java(就是一個 servlet) 文件，然後再生成一個 class。
    - RequestDispatcher 的 include() 是一個方法，包含和被包含的是兩個 servlet，兩個 .class。他們只是響應的內容在運行時合併了。
    - 作用: 把頁面分解，使用包含的方式組合在一起，這樣一個頁面中不變的部分，就是一個獨立 jsp，而我們只需處理變化的頁面。
- taglib -> 導入標籤
    - 兩個屬性:
        - prefix: 指定標籤庫在本頁面中的前綴，由我們自己來起名稱。
        - url: 指定標籤庫的位置。
        - &lt;%@taglib prefix="pre" uri="/struts-tags"%&gt; 前綴的用法 &lt;s:text&gt;

## 九大內置對象
- out: jsp 的輸出流，用來向客戶端響應。
- page: 當前 jsp 對象。他的引用類型是 Object，即真身中有如下代碼: Object page = this;
- config: 他對應真身中的 ServletConfig 對象。
- pageContext: 一個頂9個。
- request: HttpServeltRequest
- response: HttpServletResponse
- exception: Throwable
- session: HttpSession
- application: ServletContext
    - pageContext
        - 一個頂9個。
        - Servlet 中有三大域，而JSP中有四大域，他就是最後一個域對象。
            - ServletContext: 整個應用程序。
            - session: 整個繪畫(一個繪畫中只有一個用戶)
            - request: 一個請求鏈。
            - pageContext: 一個 jsp 頁面。這個域是在當前 jsp 頁面和當前 jsp 頁面中使用的標籤之間共享數據。
                - 域對象
                - 代理其他域: pageContext.setAttribute("xxx", "XXX", PageContext.SESSION_SCOPE);
                - 全域查找: pageContext.findAttribute("xxx"); 從小到大，依次查找。
                - 獲取其他8個內置對象。
                

## JSP 動作標籤
這些 jsp 的動作標籤，與 html 題中個標籤有本質的區別。
 - 動作標籤是由 tomcat (服務器)來解釋執行。他與 java 代碼一樣，都是在服務器端執行的。
 - html 由瀏覽其來執行。
 - &lt;jsp:forward&gt;: 轉發，他與 RequestDispatcher 的 forward 方法是一樣的，一個是在 Servlet 中使用，一個是在 jsp 中使用。
 - &lt;jsp:include&gt;: 包含，他與 RequestDispatcher 的 include 方法是一樣的，一個是在 Servlet 中使用，一個是在 jsp 中使用。
 - &lt;jsp:param&gt;: 他用來作為 forward 和 include 的子標籤，用來給轉發和包含的頁面傳遞參數。
