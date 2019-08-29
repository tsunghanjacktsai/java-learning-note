# dom4j 刪除

## 使用 dom4j 刪除節點
- 刪除第一個 p1 下面的元素
**步驟:**
1. 得到 document
2. 得到根結點
3. 得到第一個 p1 標籤
4. 得到第一個 p1 下面的 school 元素
5. 刪除(使用 p1 刪除 school) ->
   使用 getParent 方法得到父節點 -> 執行 remove 方法
6. 回寫 xml


```java
//刪除第一個 p1 下面的元素
	public static void delSchool() throws Exception {
		//得到 document
		Document document = Dom4jUtils.getDocument(Dom4jUtils.PATH);
		//得到根結點
		Element root = document.getRootElement();
		//得到第一個 p1 元素
		Element p1 = root.element("p1");
		//得到 p1 下面的 school 標籤
		Element school = p1.element("school");
		//刪除 school
		//通過父節點刪除
		school.getParent();//獲取 school 的父節點 p1
		p1.remove(school);
		//回寫 xml 
		Dom4jUtils.XMLWriters(Dom4jUtils.PATH, document);
	}
```
