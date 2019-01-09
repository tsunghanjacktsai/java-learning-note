# jaxp 增刪改

## 添加節點
- 步驟
1. 創建解析器工廠
2. 根據解析器工廠創建解析器
3. 解析 xml, 返回 document
4. 得到第一個 p1
-> 得到所有 p1，使用 item 方法下標得到
5. 創建 sex 標籤 createElement
6. 創建文本 createTextNode
7. 把文本添加到 sex 下面 appendChild
8. 把 sex 添加到第一個 p1 下面
9. 回寫 xml (把內存中數據寫到文檔中)

```java
//在 p1 末尾添加 <sex>Female</sex>
	public static void addSex() throws Exception {
		//創建解析器工廠
		DocumentBuilderFactory builderFactory = DocumentBuilderFactory.newInstance();
		//創建解析器
		DocumentBuilder builder = builderFactory.newDocumentBuilder();
		//得到 document
		Document document = builder.parse("src/Person.xml");
		
		//得到所有 p1
		NodeList list = document.getElementsByTagName("p1");
		//得到第一個 p1
		Node p1 = list.item(0);
		//創建標籤
		Element sex1 = document.createElement("sex");
		//創建文本
		Text text1 = document.createTextNode("Female");
		//把文本添加到 sex1 下面
		sex1.appendChild(text1);
		//把 sex1 添加到 p1 下面
		p1.appendChild(sex1);
		//回寫 xml
		TransformerFactory transformerFactory = TransformerFactory.newInstance();
		Transformer transformer = transformerFactory.newTransformer();
		transformer.transform(new DOMSource(document), new StreamResult("src/Person.xml"));	
	}
```

## 修改節點
- 步驟
1. 創建解析器工廠
2. 根據解析器工廠創建解析器
3. 解析 xml, 返回 document
4. 得到 sex -> item 方法
5. 修改 sex 裡面的值 sexTextContent 方法
6. 回寫 xml

```java
//修改第一個 p1 下面的 sex 內容是 male
	public static void modifySex() throws Exception {
		/*
		 * 1. 創建解析器工廠
		 * 2. 根據解析器工廠創建解析器
		 * 3. 解析 xml, 返回 document
		 * 4. 得到 sex -> item 方法
		 * 5. 修改 sex 裡面的值 sexTextContent 方法
		 * 6. 回寫 xml 
		 */
		//創建解析器工廠
		DocumentBuilderFactory builderFactory = DocumentBuilderFactory.newInstance();
		//創建解析器
		DocumentBuilder builder = builderFactory.newDocumentBuilder();
		//得到 document
		Document document = builder.parse("src/Person.xml");
		//得到 sex
		Node sex1 = document.getElementsByTagName("sex").item(0);
		//修改 sex 值
		sex1.setTextContent("Male");
		//回寫 xml
		TransformerFactory transformerFactory = TransformerFactory.newInstance();
		Transformer transformer = transformerFactory.newTransformer();
		transformer.transform(new DOMSource(document), new StreamResult("src/Person.xml"));
	}
```

## 刪除節點
- 步驟
1. 創建解析器工廠
2. 根據解析器工廠創建解析器
3. 解析 xml, 返回 document
4. 獲取 sex 元素
5. 獲取 sex 父節點
6. 刪除使用父節點 removeChild 方法
7. 回寫 xml 

```java
//刪除 sex 節點
	public static void delSex() throws Exception {
		/*
		 * 1. 創建解析器工廠
		 * 2. 根據解析器工廠創建解析器
		 * 3. 解析 xml, 返回 document
		 * 4. 獲取 sex 元素
		 * 5. 獲取 sex 父節點
		 * 6. 刪除使用父節點 removeChild 方法
		 * 7. 回寫 xml 
		 */
		//創建解析器工廠
		DocumentBuilderFactory builderFactory = DocumentBuilderFactory.newInstance();
		//創建解析器
		DocumentBuilder builder = builderFactory.newDocumentBuilder();
		//返回 document
		Document document = builder.parse("src/Person.xml");
		//得到 sex 元素
		Node sex1 = document.getElementsByTagName("sex").item(0);
		//得到 sex1 父節點
		Node p1 = sex1.getParentNode();
		//刪除操作
		p1.removeChild(sex1);
		//回寫 xml 
		TransformerFactory transformerFactory = TransformerFactory.newInstance();
		Transformer transformer = transformerFactory.newTransformer();
		transformer.transform(new DOMSource(document), new StreamResult("src/Person.xml"));
	}
```

##遍歷節點
- 步驟
1. 創建解析器工廠
2. 根據解析器工廠創建解析器
3. 解析 xml, 返回 document

------使用遞歸來實現-------

4. 得到根節點
5. 得到根結點的子節點
6. 得到根結點子節點的子節點

```java
//把 xml 中所有元素都打印出來
	public static void listElement() throws Exception {
		//創建解析器工廠
		DocumentBuilderFactory builderFactory = DocumentBuilderFactory.newInstance();
		//創建解析器
		DocumentBuilder builder = builderFactory.newDocumentBuilder();
		//返回 document
		Document document = builder.parse("src/Person.xml");
		//編寫一個方法遍歷操作
		list1(document);
	}
	
	public static void list1(Node node) {
		//判斷是元素類型時才打印
		if(node.getNodeType() == Node.ELEMENT_NODE) {
			System.out.println(node.getNodeName());
		}
		//得到一層子節點
		NodeList list = node.getChildNodes();
		//遍歷 list
		for(int i = 0; i < list.getLength(); i++) {
			//得到每一個節點
			Node node1 = list.item(i);
			//繼續得到 node1 子節點
			//node1.getChildNodes()
			list1(node1);
		}
	}
```
