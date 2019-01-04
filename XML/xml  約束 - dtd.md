# XML  約束 - dtd

## 定義
- 比如現在定義一個 person 的 xml 文件，只想要這個文件裡面保存人的信息，比如 name age 等，但是如果在 xml 文件中寫了一個標籤<貓>，發現可以正常顯示，因為符合語法規範。但是貓肯定不是人的信息，xml 的標籤是自訂義的，需要技術來規定 xml 中只能出現的元素，這個時候就需要**約束**。
- xml 的**約束技術**
1. dtd 約束
2. schema 約束

## dtd 
- 創建一個文件 後綴名.dtd
- 步驟
1. 看 xml 中有多少個元素，有幾個元素，在dtd文件中就寫幾個 ELEMENT 標籤
2. 判斷元素是簡單還複雜元素
   **複雜元素:** 有子元素的元素
   
   ```
   <!ELEMENT 元素名稱(子元素)>
   ```
   **簡單元素:** 沒有子元素

   ```
   <!ELEMENT 元素名稱 (#PCDATA)>
   ```
3. 需要在 xml 文件中引入 dtd 文件
   
   ```
   <!DOCTYPE 根元素名稱 SYSTEM "dtd文件的路徑">
   ```
- 打開 xml 文件使用瀏覽器打開，瀏覽器只負責校驗 xml 語法，不負責校驗約束。
- 如果想要校驗 xml 約束，需要使用工具。(Eclipse)
1. 打開開發工具
2. 創建一個項目
3. 在項目的 src 目錄下創建一個 xml 文件和一個 dtd 文件。
4. 當 xml 中引入 dtd 文件之後，比如只能出現 name age，多寫了一個標籤，就會提示出錯。
- Example
    ```
    //1.dtd
    <!ELEMENT person (name, age)>
    <!ELEMENT name (#PCDATA)>
    <!ELEMENT age (#PCDATA)>
    
    //1.xml
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE person SYSTEM "1.dtd">
    <person>
    	<name>Jack</name>
    	<age>20</age>
    	<!-- <a>111</a> -->
    </person>
    ```

## dtd 的三種引入方式
- 引入外部的 dtd 文件
    
    ```
    <! DOCTYPE 根元素名稱 SYSTEM "dtd 路徑">
    ```
- 使用內部的 dtd 文件
    ```
    <!DOCTYPE 根元素名稱 [
    	<!ELEMENT person (name, age)>
    	<!ELEMENT name (#PCDATA)>
    	<!ELEMENT age (#PCDATA)>
    ]>
    ```

- 使用外部的 dtd 文件(網路上的 dtd 文件
    
    ```
    <!DOCTYPE 根元素 PUBLIC "dtd 元素" "dtd 文檔的 URL">
    ```
    **框架 struts2** 使用配置文件長使用外部 dtd 文件。

## 使用 dtd 定義元素
- 語法
   
    ```
    <!ELEMENT 元素名 約束>
    ```
- 簡單元素: 沒有子元素的元素。
**(#PCDATA)** ->  約束 name 是字符串類型。
**EMPTY** -> 元素為空(沒有內容)。
**ANY** -> 任意。
    ```
    <!ELEMENT name (#PCDATA)>
    ```

- 複雜元素
**子元素出現的次數**
**\+** -> 表示出現一次或多次
**?** -> 表示出現零次或一次
**\*** -> 表示出現零次或多次
    
    ```
    <!ELEMENT 元素名稱 (子元素)>
    ```
子元素直接使用**逗號**進行隔開，表示元素出現的**順序**。
子元素直接使用 **|** 隔開，表示元素只能出現其中的**任意一個**。


## Example
```
<?xml version="1.0" encoding="UTF-8"?>
<!-- <!DOCTYPE person SYSTEM "1.dtd"> -->
<!DOCTYPE person [
	<!ELEMENT person (name+, age?, sex*, school)>
	<!ELEMENT name (#PCDATA)>
	<!ELEMENT age (#PCDATA)>
	<!ELEMENT sex EMPTY>
	<!ELEMENT shcool ANY>
]>
<person>
	<name>Jack</name>
	<name>Mike</name>
	<name>Vivian</name>
	<age>20</age>
	<!-- <a>111</a> -->
	<sex></sex>
	<school></school>
</person>
```
