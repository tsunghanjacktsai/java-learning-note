# dom4j 查詢

## 使用 dom4j 查詢 xml
- 查詢所有 name 元素裡面的值
**步驟:**
1. 創建解析器
2. 得到 document
3. 得到根節點 -> getRootElement() -> 返回 Element
4. 得到所有 p1 標籤
  **element(qname):** 表示獲取標籤下面的**第一個子標籤**，qname -> 標籤的名稱
  **elements(qname):** 獲取標籤下面**所有子標籤(一層)**，qname -> 標籤名稱
  **elements():** 獲取標籤下面的**所有**一層子標籤。 
5. 得到 name
6. 得到 name 裡面的值

    ```java
    //查詢 xml 中所有 name 元素的值
    	public static void selectName() throws Exception {
    		/*
    		 * 1. 創建解析器
    		 * 2. 得到 document
    		 * 3. 得到根節點
    		 * 4. 得到 p1
    		 * 5. 得到 p1 下面的 name
    		 * 6. 得到 name 裡面的值
    		 */
    		//創建解析器
    		SAXReader saxReader = new SAXReader();
    		//得到 document
    		Document document = saxReader.read("src/p1.xml");
    		//得到根結點
    		Element root = document.getRootElement();
    		//得到 p1
    		List<Element> list = root.elements("p1");
    		//遍歷 list
    		for(Element element : list) {
    			//element 是每一個 p1 的元素
    			//得到 p1 下面的 name 元素
    			Element name1 = element.element("name");
    			//得到 name 裡面的值
    			String s = name1.getText();
    			System.out.println(s);
    		}
    	}
    ```


- 查詢第一個 name 元素的值
**步驟:**
1. 創建解析器
2. 得到 document
3. 得到根節點
4. 得到第一個 p1 的元素 -> element("p1") 方法，返回 Element
5. 得到 p1 下面的 name 元素 -> element("name") 方法，返回 Element
6. 得到 name 元素裡面的值 -> getText 方法
 
    ```java
    //獲取到第一個 name 元素的值
    public static void selectSin() throws Exception {
    	//創建解析器
    	SAXReader saxReader = new SAXReader();
    	//得到 document
    	Document document = saxReader.read("src/p1.xml");
    	//得到根結點
    	Element root = document.getRootElement();
    	//得到第一個 p1
    	Element p1 = root.element("p1");
    	//得到 p1 下面的 name 元素
    	Element name1 = p1.element("name");
    	//得到 name 的值
    	String s1 = name1.getText();
    	System.out.println(s1);
    }
    ```


- 查詢第二個 name 元素的值

    ```java
    //獲取第二個 name 元素的值
    public static void selectSec() throws Exception {
    	SAXReader saxReader = new SAXReader();
    	Document document = saxReader.read("src/p1.xml");
    	Element root = document.getRootElement();
    	//得到所有 p1
    	List<Element> list = root.elements("p1");
    	//得到第二個 p1 list 集合下標從 0 開始
    	Element p2 = list.get(1);
    	//得到 p1 下面的 name
    	Element name2 = p2.element("name");
    	//得到 name 裡面的值
    	String s2 = name2.getText();
    	System.out.println(s2);
    }
    ```
