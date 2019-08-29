# dom4j 添加

## 使用
- 在第一個 p1 標籤末尾添加一個元素 
**步驟:**
1. 創建解析器
2. 得到 document
3. 得到根結點
4. 獲取第一個 p1 -> 使用 element 方法
5. 在 p1 下面添加元素 -> 在 p1 上面直接使用 addElement("標籤名稱") 方法，返回一個 Element
6. 在添加完成之後的元素下面添加文本 -> 在 sex 上直接使用 setText("文本內容") 方法
7. 回寫 xml -> 格式化 OutputFromat, 使用 createPrettyPrint 方法，表示一個漂亮的格式。
   使用類 XMLWriter 直接 new 這個類，傳遞兩個參數。 第一個參數是 xml 文件路徑 new FileOutputStream("路徑")
   第二個參數是格式化類的值


    ```java
    //在第一個 p1 元素標籤末尾添加一個元素
    	public static void addSex() throws Exception {
    		//創建解析器
    		SAXReader saxReader = new SAXReader();
    		//得到 document
    		Document document = saxReader.read("src/p1.xml");
    		//得到根結點
    		Element root = document.getRootElement();
    		//得到第一個 p1 元素
    		Element p1 = root.element("p1");
    		//在 p1 下面直接添加元素
    		Element sex1 = p1.addElement("sex");
    		//在 sex 下面添加文本
    		sex1.setText("Female");
    		//回寫 xml
    		OutputFormat format = OutputFormat.createPrettyPrint();//可以有縮進的效果
    //		OutputFormat format = OutputFormat.createCompactFormat();
    		XMLWriter xmlWriter = new XMLWriter(new FileOutputStream("src/p1.xml"), format);
    		xmlWriter.write(document);
    		xmlWriter.close();
    	}
    ```
    
## 在特定位置添加元素
- 在第一個 p1 下面的 age 標籤之前添加元素
**步驟:**
1. 創建解析器
2. 得到 document
3. 得到根結點
4. 獲取第一個 p1
5. 獲取 p1 下面所有元素 
   elements 方法 返回 list 集合
   使用 list 裡面的方法，在特定位置添加元素
   創建元素，在元素下面創建文本
6. 回寫 xml


    ```java
    //在第一個 p1 下面的 age 標籤之前添加元素
    	public static void addAgeBefore() throws Exception {
    		//創建解析器
    		SAXReader saxReader = new SAXReader();
    		//得到 document
    		Document document = saxReader.read("src/p1.xml");
    		//得到根結點
    		Element root = document.getRootElement();
    		//獲取第一個 p1
    		Element p1 = root.element("p1");
    		//獲取 p1 下面所有元素
    		List<Element> list = p1.elements();
    		//創建元素
    		Element school = DocumentHelper.createElement("school");
    		//在 school 下面創建文本
    		school.setText("Liverpool University");
    		//在特定位置添加
    		list.add(1, school);
    		//回寫 xml
    		OutputFormat format = OutputFormat.createPrettyPrint();
    		XMLWriter xmlWriter = new XMLWriter(new FileOutputStream("src/p1.xml"), format);
    		xmlWriter.write(document);
    		xmlWriter.close();
    	}
    ```
    
## dom4j 封裝方法
- 可以對得到 **document** 的操作和**回寫 xml** 的操作，封裝成**方法**。
- 可以把傳遞的**文件路徑**，封裝成一個常量。
- **好處:** 可以提高**開發速度**，可以提交代碼的**可維護性**。
  比如想要修改文件路徑(名稱)，這個時候只需要修改常量的值就好，不需要更動其他代碼。

```java
public class Dom4jUtils {
	
	public static final String PATH = "src/p1.xml";
	
	//返回 document
	public static Document getDocument(String path) {
		try {
			//創建解析器
			SAXReader reader = new SAXReader();
			//得到 document
			Document document = reader.read(path);
			return document;
		} catch (Exception e) {
			e.printStackTrace();
		}
		return null;
	}
	
	//回寫 xml
	public static void XMLWriters(String path, Document document) {
		try {
			OutputFormat format = OutputFormat.createPrettyPrint();
			XMLWriter xmlWriter = new XMLWriter(new FileOutputStream(path), format);
			xmlWriter.write(document);
			xmlWriter.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}

```
