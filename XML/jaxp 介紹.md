# jaxp 介紹

## jaxp 的 api 查看
- jaxp 是 **javase** 的一部分。
- jaxp 解析器在 jdk 的 javax.xml.parsers 包裡面。
 - 四個類: 分別是針對 dom & sax 解析使用的類。
 - **DocumentBuilder (解析器類):** **抽象類** ，不能 new，此類的實例可以從 **DocumentBuilerFactory.newDocumentBuilder()** 方法獲取。
   **解析 xml** 使用 **parse("xml路徑")** 方法，返回的是 **Document** 整個文檔。
   返回的 Document 是一個**接口**，父節點是 **Node**，如果在 Document 裡面找不到想要的方法，可以去 Node 裡面找。
   <br>
   **Document**
   **getElementByTagName(String tagname)**
   可以得到標籤，返回集合 NodeList。
   **createElement(String data)** 
   用於創建文本。
   **createTextNode(String data)**
   創建文本
   **appendChild(Node newChild)**
   把文本添加到標籤下面。
   **removeChild(Node oldChild)**
   刪除節點。
   **getParentNode()**
   獲取父節點。
   <br>
   **NodeList**
   **getLength():** 得到集合的長度。
    **getTextCotent():** 得到標籤裡面的內容。
   **item(int index):** 下標取到具體的值。
   
```java
for(int i = 0; i < list.getLength(); i++ {
    list.item(i); 
}
```
    
  <br>
 - DocumentBuilderFactory (解析器工廠): 抽象類，不能 new，可以透過 **newInstance()** 獲取 DocumentBuilderFactory 實例。
 - **SAXParser (解析器類)**
 - **SAXParseFactory (解析器工廠)**
