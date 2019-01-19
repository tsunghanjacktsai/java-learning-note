# XPATH

## 使用 dom4j 支持 XPATH 的操作
- 可以直接獲取到元素
- **第一種形式:**
  /AAA/DDD/BBB: 表示一層一層的，AAA 下面 DDD下面的 BBB
  <br>
  **第二種形式:**
  //BBB: 表示和這個名稱相同，表示只要名稱是 BBB，都可以得到。
  <br>
  **第三種形式:**
  ///*: 所有元素
  <br>
  **第四種形式:**
  BBB[1]: 表示第一個 BBB 元素
  BBB[last()]: 表示最後一個 BBB 元素
  <br>
  **第五種形式:**
  //BBB[@id]: 表示只要有 BBB 元素上面有 id 屬性，都能得到
  
  **第六種形式:**
  //BBB[@id='b1']: 表示元素名稱是 BBB, 在 BBB 上面有 id 屬性，並且 id 的屬性值是 b1

## 具體操作
- 默認情況下， dom4j 不支持 xpath
- 如果想要在 dom4j 裡面有 xpath
  - 第一步需要引入支持 xpath 的 jar 包。
  - 把 jar 包導入項目中。
- 在 dom4j 裡面提供了兩個方法，用來支持 xpath
  - selectNodes("xpath 表達式") -> 獲取多個節點
  - selectSingleNode("xpath 表達式") -> 獲取單一節點
- 使用 xpath 實現: 查詢 xml 中所有 name 元素的值
  - 所有 name 元素的 xpath 表示: **//name**
  - 使用 selectNodes("//name");

    **步驟:**
    1. 得到 document
    2. 直接使用 selectNodes("//name")方法得到所有 name 元素
    3. 遍歷 list 集合
    
    
    ```java
    //查詢 xml 中所有 name 元素的值
    	public static void test1() throws Exception {
    		//得到 document
    		Document document = Dom4jUtils.getDocument(Dom4jUtils.PATH);
    		//直接使用 selectNodes("//name")方法得到所有 name 元素
    		List<Node> list = document.selectNodes("//name");
    		//遍歷 list 集合
    		for(Node node : list) {
    			//node 是每一個 name 元素
    			//表示得到 name 元素裡面的值
    			String s = node.getText();
    			System.out.println(s);
    		}
    	}
    ```
    
- 使用 xpath 實現: 獲取第一個 p1 下面 name 的值
  - //p1[@id1='abc']/name
  - 使用到 selectSingleNode("//p1[@id1='abc']/name")
