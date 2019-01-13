# dom4j 介紹

## 使用
- dom4j，是一個組織，跟對 xml 解析，提供解析器 dom4j。
- dom4j 不是 **javase** 的一部分。
- **使用步驟:**
-  導入 dom4j 提供 jar 包
   創建一個文件夾 lib。
   複製 jar 包到 lib 下面。
   導入 external jar。
-  得到 document
    
    ```
    SAXReader reader = new SAXReader();
    Document document = reader.read(url);
    ```
- document 的父接口是 Node。(如果在 document 裡面找不到想要的方法，到 Node 裡面去找)
- document 裡面的方法 -> getRootElement(): 獲取根結點。返回的是 Element。
- Element 也是一個接口，父節點是 **Node**。
 - Element & Node 裡面的方法:
   **getParent():** 獲取父節點。
   **addElement():** 添加標籤。
