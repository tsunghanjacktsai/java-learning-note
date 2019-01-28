# Servlet

## 介紹
- Servlet 是 Javaweb 的三大組建之一，屬於**動態**資源，Servlet 的作用是**處理請求**，服務器會把接收到的請求交給 Servlet 來處理，在 Servlet 中通常需要:
 - 接收請求數據
 - 處理請求
 - 完成響應
- **單例**，一個類只有一個對象，但可能存在多個 Servlet 類
- **線程**不單例，所以效率高。
 
## 實現方式
- 三種方式:
 - 實現 javax.servlet.Servlet 接口
 - 繼承 javax.servlet.GenericServlet 類
 - 繼承 javax.servlet.http.HttpServlet 類
- Servlet.java
  Servlet 中的方法大多數不由我們來調用，而是由 **Tomcat(服務器)** 來調用。並且 Servlet 的對象也不由我們來創建，而是由 **Tomcat(服務器)** 來創建。
    
    ```java
    public void init(ServletConfig congig) throws ServletException; //生命週期方法:出生之後調用一次
    public ServletConfig getServletConfig(); 
    public service(ServletReqest req, ServletResponse res) throws ServletException, IOException; //生命週期方法:每次處理請求時都會被調用
    public String getServletInfo();
    public void destroy(); //生命週期方法:臨死(釋放資源)之前調用一次
    ```
- 瀏覽器訪問 Servlet
- 給 Servlet 指定一個 Servlet 路徑(讓 Servlet 與一個路徑綁定)
    web.xml
    
    ```java
    <servlet>
        <servlet-name>XXX</servlet-name>
        <servlet-class>servlet.ServletDemo</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>XXX</servlet-name>
        <url-pattern>/ServletDemo</url-pattern>
    <servlet-mapping>
    ```
- 瀏覽器訪問 Servlet 路徑

## ServletConfig 接口
- 一個 ServletConfig 對象，對應一段 web.xml 中的 Servlet 配置信息。
- API
 - String getServletName(): 獲取 <servlet-name> 中的內容。
 - ServletContext getServletContext(): 獲取 Servlet 上下文對象。
 - String getInitParameterNames(String name): 通過名稱獲取指定初始化參數的值。
 - Enumeration getInitParameterNames(): 獲取所有初始化參數的名稱。
 

## GenericServlet
GenericServlet 是 Servlet 接口的實現類，可以通過繼承 GenericServlet 編寫自己的 Servlet。

## HttpServlet
```
HttpServlet extends GenericServlet {
    void service(SerletRequest, ServletResponse) -> 生命週期方法
    //強轉兩個參數為 http 協議相關的類型
    //調用本類的 servic(HttpServletRequeest, HttpServletResponse)
    
    void service(HttpServletRequest, HttpServletResponse) -> 參數已經是 Http 協議相關的，使用起來更加方便
    //它會通過 request 得到當前請求的請求方式，例如: GET POST
    //根據請求方式再調用 doGet() 或 doPost() 方法
    
    void doGet() {...} -> 重寫
    
    void doPost() {...} -> 重寫
}
```
- **注意:** doGet 和 doPost 由我們自己重寫，如果沒有重寫並且他們被調用了，那麼會出現 **405**。

## 細節
- Servlet 是**非線程安全**的，說明 Servlet 工作**效率高**。不應該在 Servlet 中創建**成員變量**，因為可能會存在一個現成對這個成員變量進行寫操作，另一個現成對這個成員變量進行讀操作。
**注意:** 
 - 盡量不要創建成員變量，創建**局部變量**即可。
 - 可以創建**無狀態**成員。(EX: 人沒有名字)
 - 可以創建**有狀態**成員，但狀態必須為**唯讀**的。(EX: 只有 getter 沒有 setter)

- **讓服務器在啟動時創建 Servlet**
  默認情況下，服務器會在某個 Servlet 第一次收到請求時創建它。也可以在 web.xml 中對 Servlet 進行配置，使服務器啟動時就創建 Servlet。

    ```
    <load-on-startup>0<load-on-startup>
    ```
- **url-pattern 標籤**
  url-pattern 標籤是 servlet-mapping 標籤的子元素，用來指定 Servlet 的訪問路徑，即 URL。它必須是以 "/" 開頭。

    ```
    <servlet-mapping>
        <servlet-name>ServletDemo</servlet-name>
        <url-pattern>/ServletDemo<url-pattern>
        <url-pattern>/ServletDemo2<url-pattern> -> Servlet 綁定兩個 URL，無論訪問路徑1或2，訪問的都是 ServletDemo
    </servlet-mapping>
    ```
  可以在 url--pattern 標籤中使用**通配符**，所謂通配符就是**星號"*"**，可以匹配任何 URL 的**前綴或後綴**，使用通配符可以命名一個 Servlet 綁定一組 URL。
  
    ```
    <url-pattern>/servlet/*<url-pattern> -> /servlet/a, /servlet/b 都匹配 (路徑匹配>
    <url-pattern>*.do<url-pattern> -> /abc/def/ghi.do, /a.do 都匹配 (擴展名匹配)
    <url-pattern>/*<url-pattern> -> 匹配所有 URL。(所有都匹配)
    ```
  **注意:** 通配符不是前綴就是後綴，不能出現在 URL **中間位置**，也不能只有通配符。
 
 
## ServletContext
- **注意:** 一個項目只有一個 ServletContext 對象，可以在多個 Servlet 中來獲取這個唯一的對象，使用它可以給多個 Servlet 傳遞數據。
   與天地同壽。此對象在 Tomcat 啟動時就創建，在 Tomcat 關閉時才會死去。
- 概述:
  服務器會為每個應用創建一個 ServletContext 對象:
  - ServletContext 對象的**創建**是在**服務器啟動時**完成的。
  - ServletContext 對象的**銷毀**是在**服務器關閉時**完成。
  - SerlvetContext 對象的作用是在整個 Web 應用的**動態資源**之間共享數據。例如在 A Servlet 中向 ServletContext 對象中保存一個值，然後在 B Servlet 中就可以獲取這個值，這就是是**共享數據**。
  
- 獲取 ServletContext
 - 在 Servlet 中獲取 ServletContext 對象:
     - 在 void init(ServletConfig config) 中: ServletContext context = config.getServletContext();，ServletConfig 類的 getServletContext() 方法可以用來獲取 ServletContext 對象。
 - 在 GenericServlet 或 HttpServlet 中獲取 ServletContext 對象:
    - GenericServlet 類有 getServletContext() 方法，所以可以直接使用 this.getServletContext() 來獲取。
 - 補充: HttpSession # getServletContext()
   ServletContextEvent # getServletContext()
```java
/**
 * 演示向 ServletContext 中保存數據
 * @author asus
 */
public class ServletDemo7 extends HttpServlet {

	public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		/*
		 * 1. 獲取 ServletContext 對象
		 * 2. 調用 setAttribute() 方法完成保存數據
		 */
		ServletContext application = this.getServletContext();
		application.setAttribute("name", "Jack");
	}
}
```
```java
/**
 * 演示從 ServletContext 中獲取數據
 * @author asus
 */
public class ServletDemo8 extends HttpServlet {

	public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		/*
		 * 1. 獲取 ServletContext 對象
		 * 2. 調用 getAttribute() 方法完成獲取數據
		 */
		ServletContext application = this.getServletContext();
		String name = (String) application.getAttribute("name");
		System.out.println(name);
	}
}
```

## 域對象
- ServletContext 是 JavaWeb 四大域對象之一:
 - PageContext
 - ServletRequest
 - HttpSession
 - ServletContext
- 所有域對象都有**存取數據**的功能，因為域對象內部有一個 Map, 用來存儲數據，以下是 ServletContext 對象用來操作數據的方法:
 - void setAttribute(String name, Object value): 用來**存儲一個對象**，也可以稱之為存儲一個域屬性，EX: servletContext.setAttribute("xxx", "XXX"), 在 ServletContext 中包存一個域屬性，域屬性名稱為 xxx, 域屬性的值為 XXX。如果多次調用該方法，並且使用相同的 name, 那麼會覆蓋上一次的值，這一特性與 Map 相同。
 - Object getAttribute(String name): 用來獲取 ServletContext 中的**數據**，當前在**獲取之前需要先去存儲**才行，例如: String value = (String)servletContext.getAttribute("xxx"), 獲取名為 xxx 的域屬性。
 - void removeAttribute(String name): 用來**移除 ServletContext 中的域屬性**，如果參數 name 指定的域屬性不存在，那麼本方法甚麼都不會做。
 - Enumeration getAttributeNames(): 獲取所有域屬性的**名稱**。
 
## 獲取應用初始化參數
- 還可以使用 **ServletContext** 來獲取在 web.xml 文件中配置的**應用初始化參數**。
  **注意:** 應用初始化參數與 Servlet 初始化參數不同
- Servlet 也可以獲取初始化參數，但它是**局部參數**，也就是說，一個 Servlet 只能獲取自己的初始化參數，不能獲取別人的，即初始化參數只**為一個 Servlet 準備**。
- 可以配置公共的初始化參數，為所有 Servlet 而用，這需要使用 **ServletContext** 才能使用。

```java
/**
 * 演示 ServletContext 獲取公共的初始化參數
 * @author asus
 */
public class ServletDemo9 extends HttpServlet {

	public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		/*
		 * 1. 得到 ServletContext
		 * 2. 調用 getInitParameter(String) 得到初始化參數
		 */
		ServletContext app = this.getServletContext();
		String value = app.getInitParameter("context-param");
		System.out.println(value);
	}
}
```

## 獲取資源相關方法
- 獲取**真實路徑**
  可以使用 ServletContext 來獲取 Web 應用下的資源。
- 獲取**資源流**
  不只可以獲取資源路徑，還可以通過 ServletContext 獲取資源流，即把資源已輸入流的方式獲取。
- 獲取**指定目錄下所有資源路徑**
  還可以使用 ServletContext 獲取指定目錄下所有資源路徑，例如獲取 /WEB-INF 下所有資源的路徑。
```java
/**
 * 使用 ServletContext 獲取資源路徑
 * @author asus
 */
public class ServletDemo10 extends HttpServlet {

	/*
	 * 得到的是有盤符的路徑: C:xxxx/xxxxx/xx/
	 * C:\Program Files\Tomcat\webapps\Java_Web\index.jsp
	 */
	public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		String path = this.getServletContext().getRealPath("/index.jsp");
		System.out.println(path);
		
		/*
		 * 獲取資源的路徑後，再創建輸入流對象
		 */
		InputStream input = this.getServletContext().getResourceAsStream(path);
		
		/*
		 * 獲取當前路徑下所有資源的路徑
		 * [/WEB-INF/lib/, /WEB-INF/classes/, /WEB-INF/web.xml]
		 */
		Set<String> paths = this.getServletContext().getResourcePaths("/WEB-INF");
		System.out.println(paths);
	}
}
```

## 訪問量統計
- 一個項目中所有的資源被訪問都要對訪問量進行累加。
- 創建一個 int 類型的變量，用來保存訪問量，然後把它保存到 ServletContext 的域中。
 - 最初時，ServletContext 中沒有保存訪問量相關的屬性。
 - 當本站第一次被訪問時，創建一個變量，設置其值為 1, 保存到 ServletContext 中。
 - 當以後訪問時，就可以從 ServletContext 中獲取這個變量，然後再基礎上加 1。
 - 獲取 ServletContext 對象，查看是否存在名為 count 屬性，如果存在，說明不是第一次訪問，如果不存在，說明是第一次訪問。
- 步驟:
 - 第一次訪問: 調用 ServletContext 的 setAttribute() 傳遞一個屬性，名為 count，值為 1。
 - 第 2~N 次訪問: 調用 ServletContext 的 getAttribute() 方法獲取原來的訪問量，給訪問量加 1，再調用 ServletContext 的 setAttribute() 方法完成設置。
 
```java
/**
 * 統計訪問量
 * @author asus
 */
public class ServletExample extends HttpServlet {

	public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		/*
		 * 1. 獲取 ServletContext 對象 2. 從 ServletContext 對象中獲取名為 count 的屬性 3.
		 * 如果存在，給訪問量加 1，然後保存回去 4. 如果不存在，說明是第一次訪問，向 ServletContext 中保存名為 count
		 * 的屬性，值為 1
		 */
		ServletContext app = this.getServletContext();
		Integer count = (Integer) app.getAttribute("count");
		if (count == null) {
			app.setAttribute("count", 1);
		} else {
			app.setAttribute("count", count + 1);
		}
		System.out.println(count);
		
		/*
		 * 向瀏覽器輸出
		 * 需要使用響應對象
		 */
		PrintWriter pw = response.getWriter();
		pw.print("<h1>" + count + "</h1>");
	}
}
```

## 獲取類路徑下的資源
- 獲取類路徑資源，類路徑對一個 JavaWeb 項目而言，就是 /WEB-INF/classes 和 /WEB-INF/lib/每個 jar 包。
```java
import java.io.IOException;
import java.io.InputStream;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.commons.io.IOUtils;

/**
 * 演示獲取類路徑下的資源
 * 
 * @author asus
 */
public class ServletDemo11 extends HttpServlet {

	public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		/*
		 * 1. 得到 ClassLoader -> 先得到 Class, 再得到 ClassLoader 2. 調用
		 * getResourceAsStream()，得到一個 InputStream
		 */
		/*
		 * ClassLoader cl = this.getClass().getClassLoader(); 
		 * InputStream input = cl.getResourceAsStream("Java_Web/servlet/a.txt"); //相對 /classes
		 */

		Class c = this.getClass();
		//相對 .class 文件所在目錄
//		InputStream input = c.getResourceAsStream("a.txt");
		
		//相對 classes 下
		InputStream input = c.getResourceAsStream("/a.txt");

		String s = IOUtils.toString(input); // 讀取輸入流內容，轉換成字符串返回
		System.out.println(s);
	}
}
```
