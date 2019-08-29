# sax 的解析

## 原理
- 解析 xml 有兩種技術: **dom** & **sax**
- 根據 xml 的層級結構在內存中分配一個**樹型結構**。
  把 xml 中**標籤、屬性、文本**封裝成**對象**。
- sax 方式: **事件驅動**，邊讀邊解析。
- 在 javax.xml.parsers 包裡面
 - **SAXParser:** 此類實例可以從 **SAXParserFactory.newSAXParser()** 獲得。
   **parse(File f, DefaultHandler dh)** -> 兩個參數，第一個參數為**xml路徑**，第二個參數為**事件處理器**。
  - **SAXParserFactory:** 實例 newInstance() 方法得到。
- 當解析到開始標籤的時候，自動執行 startElement方法。
  當解析到文本的時候，自動執行 characters 方法。
  當解析到結束標籤的時候，自動執行 endElement 方法。
- 圖片解析

![](leanote://file/getImage?fileId=5aa54dcf303c1f2951000000)

## 使用 jaxp 的 sax 方式解析 xml
- sax 方式不能實現增刪改操作，只能實現查詢操作。
- 打印整個文檔
**步驟:**
1. 創建解析器工廠
2. 獲取解析器
3. 執行 parse 方法
4. 自己創建一個類，繼承 DefaultHandler
5. 重寫類裡面的三個方法

   
    ```java
    public class SaxTest {
    
    	public static void main(String[] args) throws Exception {
    		//創建解析器工廠
    		SAXParserFactory saxParserFactory = SAXParserFactory.newInstance();
    		//獲取解析器
    		SAXParser saxParser = saxParserFactory.newSAXParser();
    		//執行 parse 方法
    		saxParser.parse("src/p1.xml", new MyDefault());
    	}
    }
    
    class MyDefault extends DefaultHandler {
    
    	@Override
    	public void startElement(String uri, String localName, String qName, Attributes attributes) throws SAXException {
    		System.out.print("<" + qName + ">");
    	}
    
    	@Override
    	public void characters(char[] ch, int start, int length) throws SAXException {
    		System.out.print(new String(ch, start, length));
    	}
    
    	@Override
    	public void endElement(String uri, String localName, String qName) throws SAXException {
    		System.out.print("</" + qName + ">");
    	}
    }
    ```

- 獲取到所有的 name 元素的值
**步驟:**
1. 定義一個成員變量 flag = false
2. 判斷開始方法是否是 name 元素，如果是，把 flag 設置為 true。
3. 如果 flag 值是 true，在 characters 方法裡面打印內容。
當執行到結束方法時，把 flag 值設置為 false。

    ```java
    public class SaxTest {
    
    	public static void main(String[] args) throws Exception {
    		/*
    		 * 1. 創建解析器工廠
    		 * 2. 獲取解析器
    		 * 3. 執行 parse 方法
    		 * 4. 自己創建一個類，繼承 DefaultHandler
    		 * 5. 重寫類裡面的三個方法
    		 */
    		//創建解析器工廠
    		SAXParserFactory saxParserFactory = SAXParserFactory.newInstance();
    		//獲取解析器
    		SAXParser saxParser = saxParserFactory.newSAXParser();
    		//執行 parse 方法
    		saxParser.parse("src/p1.xml", new MyDefault_2());
    	}
    }
    
    //實現獲取所有 name 元素的值
    class MyDefault_2 extends DefaultHandler {
    
    	boolean flag = false;
    	
    	@Override
    	public void startElement(String uri, String localName, String qName, Attributes attributes) throws SAXException {
    		//判斷 qName 是否是 name 元素
    		if("name".equals(qName)) {
    			flag = true;
    		}
    	}
    
    	@Override
    	public void characters(char[] ch, int start, int length) throws SAXException {
    		//當 flag 值是 true 時，表示解析到 name 元素
    		if(flag == true) {
    			System.out.println(new String(ch, start, length));
    		}
    	}
    
    	@Override
    	public void endElement(String uri, String localName, String qName) throws SAXException {
    		//把 flag 設置為 false，表示 name 元素結束
    		if("name".equals(qName)) {
    			flag = false;
    		}
    	}
    }
    ```
