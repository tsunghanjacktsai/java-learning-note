# EL 表達式

## 概述
- 是 JSP 內置的表達式語言。
    - jsp2.0 開始，不讓再使用 java 腳本，而是使用 el 表達式和動態標籤來替代 java 腳本。
    - el 替代的是<%= ... %>, 也就是說，el只能做輸出。
    
- EL 表達式來讀取四大域
    - ${xxx}, 全域查找名為 xxx 的屬性，如果不存在，輸出空字符串，而不是 null。
    - {pageScope.xxx}, {requestScope.xxx}, {sessionScope.xxx}, {applicationScope.xxx}, 指定域獲取屬性。

- JavaBean 導航    
- EL 可以輸出的東西都在11個內置對象中。(11個內置對象，其中10個是 Map, pageContext 不是 map，他就是 PageContext 類型)
    - param: 對應參數，他是一個 map, 其中 key 參數名，value 是參數值，是用於單值的參數。
    - paramValues: 對應參數，他是一個 map, 其中 key 參數名，value 是**多個**參數值，是用於**多值**的參數。
    - header: 對應請求頭，他是一個 Map, 其中 key 表示頭名稱，value 是單個頭值，是用於單值請求頭。
    - headerValues: 對應請求頭，他是一個 Map, 其中 key 表示頭名稱，value 是**多個**頭值，是用於**多值請求頭**。
    - initParam: 獲取&lt;context-param&gt; 內的參數。
        ```xml
        <context-param>
            <param-name>xxx</param-name>
            <param-value>XXX</param-value>
        </context-param>
        <context-param>
            <param-name>yyy</param-name>
            <param-value>YYY</param-value>
        <context-param>
        ${initParam.xxx}
        ```

    - cookie: Map&lt;String, Cookie&gt; 類型，其中 key 是 cookie 的 name, value 是 cookie 對象。${cookie.username.value}
    
    - pageContext: 他是 PageContext 類型。${pageContext.request}
    
        ```JSP
        <a href="${pageContext.request.contextPath} }/el/a.jsp">點擊這裡</a>
          	<form action="${pageContext.request.contextPath }/cookie/a.jsp" method="post">
          		<input type="submit" value="xxx">
          	</form>
        ```
        
## EL 函數庫 (由 JSTL 提供的)
- 導入標籤庫: &lt;%@ tablib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions"%&gt;
	String toUpperCase(String input)：把參數轉換成大寫
	String toLowerCase(String input)：把參數轉換成小寫
	int indexOf(String input, String substring)：從大串，輸出小串的位置！
	boolean contains(String input, String substring)：查看大串中是否包含小串
	boolean containsIgnoreCase(String input, String substring)：忽略大小寫的，是否包含
	boolean startsWith(String input, String substring)：是否以小串為前綴
	boolean endsWith(String input, String substring)：是否以小串為後綴
	String substring(String input, int beginIndex, int endIndex)：截取子串
	String substringAfter(String input, String substring)：獲取大串中，小串所在位置後面的字元串
	substringBefore(String input, String substring)：獲取大串中，小串所在位置前面的字元串
	String escapeXml(String input)：把input中“<”、">"、"&"、"'"、"""，進行轉義
	String trim(String input)：去除前後空格
	String replace(String input, String substringBefore, String substringAfter)：替換
	String[] split(String input, String delimiters)：分割字元串，得到字元串數組
	int length(Object obj)：可以獲取字元串、數組、各種集合的長度。
	String join(String array[], String separator)：聯合字元串數組。

## 自訂義函數庫
- 寫一個 java 類，類中可以定義 0~N 個方法，但必須是 static, 且有返回值。
- 在 WEB-INF 目錄下創建一個 tld 文件
    ```xml
    <function>
        <name>fun</name>
        <function-class>cn.itcast.fn.MyFunction</function-class>
        <function-signature>java.lang.String fun()</function-signature>
      </function>
    ```
- 在 jsp 頁面中導入標籤庫
    ```JSP
    <%@ taglib prefix="it" uri="/WEB-INF/tlds/itcast.tld" %>
    ```
- 在 jsp 頁面中使用自定義的函數: ${it:fun() }
