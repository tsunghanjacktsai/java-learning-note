# schema 約束

## 介紹
- dtd 語法: 
```xml
<!ELEMENT 元素名稱 約束>
```
- schema 符合 xml 的語法，xml 語句。
- 一個 xml 可以有多個 schema，多個 schema 使用**名稱空間**區分。(類似於 java **包名**)
- dtd 裡面有 PCDATA 類型，但是在 schema 裡面可以支持**更多的數據類型**。 
-> 比如 年齡 只能是整數，在 schema 可以直接定義一個整數類型。
- schema 語法更加複雜，schema 不能替代 dtd。

## 使用
- 創建一個 schema 文件 -> 後綴名是 xsd -> 根結點名稱為 schema
- 在 schema 文件裡
   **屬性:** xmlns="http://www.w3.org/2001/XMLSchema" -> 表示當前 xml 文件是一個約束文件。
   targetNamespace="http://www.example.org/1" -> 使用 schema 約束文件，直接通過這個地址引入約束文件

**步驟:**
- 看 xml 中有多少個元素 -> element
- 複雜元素

    ```xml
    <element name="person">
    	<complexType>
    		<sequence>
    		    子元素
    		</sequence>
    	</complexType>
    </element>
    ```
- 簡單元素(寫在複雜元素的中間)
    ```xml
     <element name="person">
        	<complexType>
        		<sequence>
        		    <element name="name" type="string"></element>
        			<element name="age" type="int"></element>
        		</sequence>
        	</complexType>
        </element>    			
    ```

- 在被約束文件裡面引入約束文件
    ```xml
    <person xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns="http://www.example.org/1"
    xsi:schemaLocation="http://www.example.org/1 1.xsd">
    ```
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" -> 表示 xml 是一個**被約束文件**
    
    "http://www.example.org/1" -> 是約束文檔裡面 **targetNamespace**

    xsi:schemaLocation="http://www.example.org/1 1.xsd" -> targetNamespace 空格 **約束文檔的地址路徑**
    
- **sequence 標籤:** 元素出現順序。
- **all 標籤:** 元素只能出現一次。
- **choice 標籤:** 元素只能出現其中一個。
- **maxOccurs="unbounded":** 表示出現次數沒限制。
    
    ```xml
    <element name="name" type="string" maxOccurs="unbounded"></element>
    ```
- **any 標籤:** 表示任意元素。
- **attribute 標籤:** 可以**約束屬性**，寫在複雜元素裡面，complexType 標籤之前。
  name: 屬性名稱。
  type: 屬性類型。
  use: 屬性是否必須出現。
    
    ```xml
    <attribute name="id1" type="int" use="required"></attribute>
    ```
- 複雜的 schema 約束
  引入多個 schema 文件，可以給每個起一個**別名**
  想要引入特定約束文件裡的元素，使用 **別名:元素名稱**。
    
    ```xml
    <company xmlns="http://www.example.org/company"
    xmlns:dept="http://www.example.org/department"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.example.org/company company.xsd http://www.example.org/department department.xsd">
    <employee age="30">
		<!-- 部門名稱 -->
		<dept:name>100</dept:name>
		<!-- 員工名稱 -->
		<name>Jack</name>
	</employee>
    ```
