# JSTL

## 概述
JSTL 是 apache 對 EL 表達式的擴展(也就是說 JSTL 依賴 EL)，JSTL 是標籤語言。使用時須導包。

## JSTL 標籤庫
JSTL 一共包含四大標籤庫。
- core: 核心標籤庫。
- fmt: 格式化標籤庫。
- sql: 數據庫標籤庫(過時)。
- xml: xml 標籤庫(過時)。

## 使用 taglib 指令導入標籤庫
- jar 包
- 在 jsp 頁面中: &lt;%@taglib prefix="前綴" uri="路徑"&gt;

## core 標籤庫常用標籤
- out & set
    - &lt;c:out&gt;: 輸出。
        - value: 可以是字符串常量，也可以是 EL 表達式。
        - default: 當要輸出的內容為 null 時，會輸出 default 指定的值。
        - escapeXml: 默認值為 true, 表示轉義。
    - &lt;c:set&gt;: 創建域的屬性。
        - var: 變量名。
        - value: 變量值，可以是 EL 表達式。
        - scope: 域，默認為 page, 可選值: page, request, session, application
- remove
    - &lt;remove&gt;: 刪除域變量。
        - var: 變量名。
        - scope: 如果不給出 scope, 表示刪除所有域中該名稱的變量，如果指定了域，那麼只刪除該域的變量。
- url
    - value: 指定一個路徑，他會在路徑前面自動添加項目名。
        - &lt;c:url value="/index.jsp"/&gt;, 他會輸出 /Java_Web/index.jsp
    - 子標籤: &lt;c:param&gt;, 用來給 url 後面添加參數，例如:

        ```JSP
        <c:url value="/index.jsp">
            <c:param name="username" value="Jack"/> <!-- 可以對參數進行 url 編碼 -->
        </c:url>
        //結果為: /Java_Web/index.jsp?username=Jack
        ```
    - var: 指定變量名，一旦添加了這個屬性，那麼 url 標籤就不會再輸出到頁面，而是把生成 url 保存到域中。
    - scope: 他與 var 一起使用，用來保存 url。
- if: 對應 java 中的 if 語句
    - &lt;c:if test="布爾類型"&gt;...&lt;/c:if&gt;, 當 test 為值時，執行標籤體內容。
- choose: 他對應 java 中的 if/else if/.../else
    - 例如:

    ```JSP
    <c:choose>
        <c:when test="">...</c:when>
        <c:when test="">...</c:when>
        <c:when test="">...</c:when>
        ...
        <c:otherwise></c:otherwise>
    </c:choose>
    //等同於
    if(...) {
    } else if(...) {
    } else if(...) {
    } else if(...) {
    } ...
    else {...}
    ```
- forEach: 他用來循環遍歷數組、集合。他還可以用計數方式來循環。
  計數方式:
    ```JSP
    for(int i = 0; i <= 10; i++) {
        ...
    }
    
    <c:forEach var="i" begin="1" end="10">
        ${i}
    </c:forEach>
    ```
  屬性:
    - var: 循環變量。
    - begin: 設置循環變量從多少開始。
    - end: 設置循環變量到多少結束。
    - step: 設置步長。等同於 java 中的 i++, 或 i+=2, step 默認為1。
  用來輸出數組、集合。

        ```JSP
        <c:forEach items="${strs }" var="str">
            ${str }</br>
        </c:forEach>
        //等同於
        for(String str: strs) {
            ...
        }
        ```
  屬性:
    - items: 指定要循環誰，他可以是一個數組或一個集合。
    - var: 把數組或集合中的每個元素賦值給 var 指定的變量。
    

## fmt 標籤庫常用標籤
fmt 標籤庫是用來格式化輸出的，通常需要格式化的是時間和數字。

- 格式化時間:
    
    ```JSP
    <%
    	Date date = new Date();
    	pageContext.setAttribute("d", date);
    %>
    <fmt:formatDate value="${d }" pattern="yyyy-MM-dd HH:mm:ss"/>
    ```
- 格式化數字:

    ```JSP
    <%
    	double d1 = 3.5;
    	double d2 = 4.4; 
    	pageContext.setAttribute("d1", d1);
    	pageContext.setAttribute("d2", d2);
    %>
    <fmt:formatNumber value="${d1 }" pattern="0.00"/><br/>
    <fmt:formatNumber value="${d2 }" pattern="#.##"/>
    
    ```
