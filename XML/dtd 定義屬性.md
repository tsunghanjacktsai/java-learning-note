# dtd 定義屬性
## 使用1
- 語法
   
    ```xml
    <!ELEMENT 元素名 約束>
    ```
- 簡單元素: 沒有子元素的元素。
**(#PCDATA)** ->  約束 name 是字符串類型。
**EMPTY** -> 元素為空(沒有內容)。
**ANY** -> 任意。
    ```xml
    <!ELEMENT name (#PCDATA)>
    ```

- 複雜元素
**子元素出現的次數**
**\+** -> 表示出現一次或多次
**?** -> 表示出現零次或一次
**\*** -> 表示出現零次或多次
    
    ```xml
    <!ELEMENT 元素名稱 (子元素)>
    ```
子元素直接使用**逗號**進行隔開，表示元素出現的**順序**。
子元素直接使用 **|** 隔開，表示元素只能出現其中的**任意一個**。


- Example
    ```xml
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

## 使用2
- 語法
    
    ```xml
    <!ATTLIST 元素名稱 屬性名稱 屬性類型 屬性約束>
    ```
- 屬性類型
1. **CDATA:** 表示屬性的取值為普通的文本字符串。
2. **ENUMERATED (dtd 沒有此關鍵字):** 表示枚舉，只能在一定的範圍內出現值，但是只能每次出現其中一個。如: (雞肉 | 牛肉 | 豬肉 | 魚肉)、紅綠燈
3. **ID:** 表示屬性的取值不能重複，屬性的值只能由字母，下滑線開始，不能出現**空白字符**。

- 屬性約束
1. **#REQUIRED:** 表示該屬性**必須出現**。
2. **#IMPLIED:** 表示該屬性**可有可無**。
3. **#FIXED:** 表示屬性的取值為一個**固定值**。語法 -> #FIXED "固定值"
4. 直接值: 表示屬性的取值為**該默認值**，不寫屬性，使用直接值，寫了屬性，則使用設置的值。
**語法**
```xml
<!APPLIED school ID5 CDATA "默認值">
```

## 實體定義
- 語法
```xml
<!ENTITY 實體名稱 "實體值">
<!ENTITY TEST "HAHA">
```
- 使用實體 &實體名稱; -> &TEST;
- **注意:** 定義實體需要寫在內部 dtd 裡面，如果寫在外部的 dtd 裡面，有某些瀏覽器下，內容得不到。
