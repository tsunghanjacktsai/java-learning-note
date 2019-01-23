# HTTP 協議

## 介紹
HTTP(Hypertext transport protocol)，即超文本傳輸協議。這個協議規定**瀏覽器**和**萬維網服務器**之間互相通信的規則。
規定了客戶端發送給服務器的內容格式，也規定了服務器發送給客戶端的內容格式。客戶端發送給服務器的格式較**請求協議**，服務器發送給客戶端的格式較**響應協議**。

## 請求協議
- 格式
```
請求首行:
請求頭信息;
空行;
請求體;
```
- GET請求
    ```
    GET /Hello/index.jsp HTTP/1.1
    Host: localhost
    User-Agent: Mozilla/5.0 (Windows NT 5.1; rv:5.0) Gecko/20100101 Firefox/5.0
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
    Accept-Language: zh-cn,zh;q=0.5
    Accept-Encoding: gzip, deflate
    Accept-Charset: GB2312,utf-8;q=0.7,*;q=0.7
    Connection: keep-alive
    Cookie: JSESSIONID=369766FDF6220F7803433C0B2DE36D98
    ```
 -  GET /Hello/index.jsp HTTP/1.1: GET 請求, 請求服務器路徑為 /Hello/index.jsp, 協議為 1.1。
 -  Host: localhost: 請求的主機名為 localhost。
 -  User-Agent: Mozilla/5.0 (Windows NT 5.1; rv:5.0) Gecko/20100101 Firefox/5.0
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8: 與瀏覽器和 OS 相關的信息。有些網站會顯示用戶的系統版本和瀏覽器版本信息，這都是通過獲取 User-Agent 頭信息而來的。
 - Accept-Language: zh-cn,zh;q=0.5: 當前客戶端支持的語言。
 -  Accept-Encoding: gzip, deflate: 支持的壓縮格式，數據在網絡上傳遞時，可能服務器會把數據壓縮後再發送。
 -   Accept-Charset: GB2312,utf-8;q=0.7,*;q=0.7: 客戶端支持的編碼。
 -   Connection: keep-alive: 客戶端支持的鏈接方式，保持一段時間鏈接，默認為 3000ms。
 -   Cookie: JSESSIONID=369766FDF6220F7803433C0B2DE36D98: 因為不是第一次訪問這個地址，所以會再請求中把上一次服務器響應中發送過來的 Cookie 再請求中一併發送出去。
 
- POST 請求
    ```
    POST /Hello/index.jsp HTTP/1.1
    Accept: image/gif, image/jpeg, image/pjpeg, image/pjpeg, application/msword, application/vnd.ms-excel, application/vnd.ms-powerpoint, application/x-ms-application, application/x-ms-xbap, application/vnd.ms-xpsdocument, application/xaml+xml, */*
    Referer: http://localhost:8080/hello/index.jsp
    Accept-Language: zh-cn,en-US;q=0.5
    User-Agent: Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Trident/4.0; InfoPath.2; .NET CLR 2.0.50727; .NET CLR 3.0.4506.2152; .NET CLR 3.5.30729)
    Content-Type: application/x-www-form-urlencoded
    Accept-Encoding: gzip, deflate
    Host: localhost:8080
    Content-Length: 13
    Connection: Keep-Alive
    Cache-Control: no-cache
    Cookie: JSESSIONID=E365D980343B9307023A1D271CC48E7D
    
    keyword=hello
    ```
 - POST 請求是可以有體的，GET 請求不能有請求體。
 - Referer: http://localhost:8080/hello/index.jsp: 請求來自哪個頁面。可用於做**統計工作**，也可以用於做**防盜鏈**。
 - Content-Type: application/x-www-form-urlencoded: 表單的數據類型，說明會使用 url 格式編碼數據， usl 編碼的數據都是以 "%" 為前綴，後面跟隨兩位 16 進制。
 - Content-Length: 13: 請求體的長度。
 - keyword=hello: 請求體內容。hello 是在表單中輸入的數據，keyword 是表單字段的名字。

- 響應協議
- 格式

    ```
    響應首行;
    響應頭信息;
    空行;
    響應體;
    ```
- 響應內容是由服務器發送給瀏覽器的內容，瀏覽器會根據響應內容來顯示。
    
    ```
    HTTP/1.1 200 OK
    Server: Apache-Coyote/1.1
    Content-Type: text/html;charset=UTF-8
    Content-Length: 724
    Set-Cookie: JSESSIONID=C97E2B4C55553EAB46079A4F263435A4; Path=/hello
    Date: Thu, 23 Mar 2018 12:01:30 GMT
    
    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
    <html>
      <head>
        <base href="http://localhost:8080/hello/">
        
        <title>My JSP 'index.jsp' starting page</title>
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
    <form action="" method="post">
      关键字：<input type="text" name="keyword"/>
      <input type="submit" value="提交"/>
    </form>
      </body>
    </html>
    ```
 - HTTP/1.1 200 OK：響應協議為 HTTP1.1，狀態碼為 200，表示請求成功，OK 是對狀態碼的解釋。
 - Server: Apache-Coyote/1.1：服務器的版本信息。
 - Content-Type: text/html;charset=UTF-8：響應體使用的編碼格式為 UTF-8。
 - Content-Length: 724: 響應體長度(字節)。
 - Set-Cookie: JSESSIONID=C97E2B4C55553EAB46079A4F263435A4; Path=/hello：響應給客戶端的 Cookie。
 - Date: Thu, 23 Mar 2018 12:01:30 GMT：響應的時間，中國會有 8 小時時差。
 
- 響應碼
 - **200:** 請求成功，瀏覽器會把響應體內容(通常是 html) 顯示在瀏覽器中。
 - **404:** 請求的資源沒有找到，說明客戶端錯誤的請求不存在的資源。
 - **500:** 請求資源找到，但服務器出現錯誤。
 - **302:** 重定向，當響應碼為 302 時，表示服務器要求瀏覽器再發一個請求，服務器會發送一個響應頭 Location，他指定了新請求的 URL 地址。
 - **304:** 當用戶第一次請求 index.html 時，再請求中包含一個名為 If-Modified-Since 請求頭，他的直是第一次請求時服務器通過 Last-Modified 響應頭發送給瀏覽器的值，即 index.html最後修改時間。If-Modified-Since 請求就是再告訴服務器，我這裡瀏覽器緩存的 index.html 最後修改時間是這個，你看看現在的 index.html 最後修改時間是不是這個。而服務器端會獲取 If-Modified-Since 值，與 index.html 的當前最後修改時間比對，如果相同，服務器會發響應碼**304**，表示 index.html 與瀏覽器上次緩存的相同，無須再次發送，瀏覽器可顯示自己的緩存頁面，如果比對不同，那麼說明 index.html 已經做了修改，服務器會響應 200。
 
- 其他響應頭
 - 告訴瀏覽器**不要緩存**的響應頭:
   Expires-1;
   Cache-Control: no-cache;
   Pragma: no-cache;
 - **自動刷新**響應頭，瀏覽器會在 3 秒之後請求目標網址:
   Refresh:3;url=http://www.baidu.com
