# dom4j 修改

## 使用 dom4j 實現修改節點的操作
- 修改第一個 p1 下面的 age 元素的值
**步驟:**
1. 得到 document
2. 得到根結點，然後得到第一個 p1 元素
3. 得到第一個 p1 下面的 age -> element("") 方法
4. 修改值 40 -> 使用 setText("文本內容") 方法
5. 回寫 xml

```java
//修改 p1 下面的 age 元素的值
	public static void modifyAge() throws Exception {
		//得到 document
		Document document = Dom4jUtils.getDocument("src/p1.xml");
		//得到第一個 p1
		Element root = document.getRootElement();
		//得到第一個 p1
		Element p1 = root.element("p1");
		//得到 p1 下面的 age
		Element age = p1.element("age");
		//修改 age 的值
		age.setText("40");
		//回寫 xml
		Dom4jUtils.XMLWriters(Dom4jUtils.PATH, document);
	}
```
