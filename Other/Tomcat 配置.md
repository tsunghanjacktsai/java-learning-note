# Tomcat 配置

## 配置外部應用
- conf/server.xml: 打開 **server.xml** 文件，找到 **<Host>** 元素，在其中添加 <Context> 元素。
server.xml
```xml
<Host name="localhost" addBase="webapps" unpackWARs="true" autoDeploy="true">
  <Context path="Hello" docBase="C:/Hello" />
<Host>
```
1. path: 指定當前應用的名稱。
2. docBase: 指定應用的物理位置。
3. 瀏覽器訪問路徑: http://localhost:8080/Hello/index.html

- conf/catalina/localhost: 在該目錄下創建 Hello.xml, 在該文件中編寫 <Context> 元素
```xml
<Context docBase="C:/Hello" />
```
1. 文件名: 指定當前應用的名稱。
2. docBase: 指定應用的物理位置。
3. 瀏覽器訪問路徑: http://localhost:8080/Hello/index.html
