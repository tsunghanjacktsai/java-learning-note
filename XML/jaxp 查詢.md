# jaxp 查詢

## 使用 jaxp 實現查詢操作
- 查詢 xml 中所有的 name 元素的值
- 步驟:
1. 創建解析器工廠
DocumentBuilderFactory.newInstance();
2. 根據解析器工廠創建解析器
builderFactory.newDocumentBuilder();
3. 解析 xml 返回 document
Document document = builder.parse("src/Person.xml");
4. 得到所有的 name 元素
使用 document.getElementByTagName("name");
5. 返回集合，遍歷集合，得到每一個 name 元素
遍歷 getLength() item()
得到元素裡面的值 使用 getTextContent()

```java
public static void searchAll() throws Exception {
		//查詢所有 name 元素的值
		/*
		 * 1. 創建解析器工廠
		 * 2. 根據解析器工廠創建解析器
		 * 3. 解析 xml 返回 document
		 * 4. 得到所有的 name 元素
		 * 5. 返回集合，遍歷集合，得到每一個 name 元素
		 */
		
		//創建解析器工廠
		DocumentBuilderFactory builderFactory = DocumentBuilderFactory.newInstance();
		//根據解析器
		DocumentBuilder builder = builderFactory.newDocumentBuilder();
		//解析 xml 返回 document
		Document document = builder.parse("src/Person.xml");
		//得到所有的元素
		NodeList list = document.getElementsByTagName("name");
		//遍歷集合
		for(int x = 0; x < list.getLength(); x++) {
			Node name1 = list.item(x);//得到每一個 name 元素
			//得到 name 元素裡面的值
			String s = name1.getTextContent();
			System.out.println(s);
		}
	}
```

## 查詢 xml 中第一個 name 元素的值
- 步驟
1. 創建解析器工廠
2. 根據解析器工廠創建解析器
3. 解析 xml, 返回 document
4. 得到所有 name 元素
5. 使用返回集合，里面方法 item，下標獲取具體元素
**NodeList.item (下標):** 集合下標從 0 開始
6. 得到具體的值，使用 getTextContent 方法

```java
public static void searchSin() throws Exception {
		/*
		 * 1. 創建解析器工廠
		 * 2. 根據解析器工廠創建解析器
		 * 3. 解析 xml, 返回 document
		 * 4. 得到所有 name 元素
		 * 5. 使用返回集合，里面方法 item，下標獲取具體元素
		 * 6. 得到具體的值，使用 getTextContent 方法
		 */
		//創建解析器工廠
		DocumentBuilderFactory builderFactory = DocumentBuilderFactory.newInstance();
		//創建解析器
		DocumentBuilder builder = builderFactory.newDocumentBuilder();
		//解析 xml, 得到 document
		Document document = builder.parse("src/Person.xml");
		//得到所有的 name 元素
		NodeList list = document.getElementsByTagName("name");
		//使用下標得到第一個元素
		Node name1 = list.item(0);
		//得到 name 裡具體的值
		String s1 = name1.getTextContent();
		System.out.println(s1);
	}
```
