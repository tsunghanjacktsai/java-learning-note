# JUnit

## 使用
- 單元測試
- 測試對象: 一個類中的方法。
- JUnit 不是 Javase 的一部分，想要使用需導入**jar包**。(myEclipse 中自帶jUnit 的 jar 包)
- 單元測試方法得時候，方法命名規則 **public void 方法名() {}** -> 不能有參數
- 使用註解方式運行測試方法，在方法的上面
 - @Test: 表示方法進行單元測試

     ```java
    @Test
    	public void testAdd() {
    		TestJUnit test01 = new TestJUnit();
    		test01.testAdd(100, 200);
    	}
    ```
    
- 當出現綠色條，表示方法測試通過。
  當出現了紅棕色條，表示方法測試不通過。
- 要運行類中的多個測試方法，點擊類中的其他位置，run as -- jUnit test
- @Ignore: 表示這個方法不進行單元測試。
- @Before: 在每一個方法之前運行。
- @After: 在每一個方法之後運行。
