# FileWriter

## 演示
- **需求:** 將一些文字存儲到硬盤一個文件中。
- **思路:** 如果要操作文字數據，建議優先考慮字符流。
          而且要將數據從內存寫到硬盤上，要使用字符流中的輸出流(Writer)。
          硬盤的數據基本體現是文件。希望可以找到一個可以操作文件的 Writer。
          ----> **FileWriter**。
- **步驟:** 
1. 創建一個可以往文件中寫入字符數據的字符輸出流對象，
   既然是往一個文件中寫入文字數據，那麼再創建對象時，就必須明確該文件(用於存儲數據的目的地)
   如果文件不存在，則會自動創建。
   如果文件存在，則會被覆蓋。
   如果構造函數中加入 true，可以實現對文本續寫。(false 則不續寫)
2. 調用 Writer 對象中的 write(String) 方法寫入數據，
   其實數據寫入到"臨時存儲緩衝區"中。
3. 進行刷新，將數據寫入目的地中。

```
import java.io.FileWriter;
import java.io.IOException;

public class FileWriterDemo {
	
	private static final String LINE_SEPERATOR = System.getProperty("line.separator");

	public static void main(String[] args) throws IOException {

		/*
		 * 創建一個可以往文件中寫入字符數據的字符輸出流對象，
		 * 
		 * 既然是往一個文件中寫入文字數據，那麼再創建對象時，就必須明確該文件(用於存儲數據的目的地)
		 * 
		 * 如果文件不存在，則會自動創建。
		 * 如果文件存在，則會被覆蓋。
		 */
		FileWriter fw = new FileWriter("demo.txt");
		
		/*
		 * 調用 Writer 對象中的 write(String) 方法寫入數據
		 * 
		 * 其實數據寫入到"臨時存儲緩衝區"中
		 */
		
		fw.write("Hello\r\nWorld\r\n");//換行
		fw.write("Hello" + LINE_SEPERATOR + "World");//換行
		
		/*
		 * 進行刷新，將數據寫入目的地中。
		 */
		
		fw.flush();
		
		/*
		 * 關閉流，關閉資源。在關閉前會調用 flush 刷新緩衝中的數據到目的地。
		 */
		
		fw.close();
		
//		fw.write("HaHa");//已關閉 -> java.io.IOException: Stream closed
	}
}
```
文本顯示結果
```
Hello
World
Hello
World
```
