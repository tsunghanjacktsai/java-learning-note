# Runtime 類

## 使用
- 沒有構造方法摘要，說明該類不可以創建對象。
又發現還有非靜態的方法。說明該類應該提供靜態的返回該類對象的方法。
而且只有一個，說明 Runtime 類使用了**單例設計模式**。
- execute: 執行 xxx.exe


## 演示
```
import java.io.IOException;

/**
 * @author asus
 * @throws IOException
 * @throws InterruptedException
 */
public class RuntimeDemo {

	public static void main(String[] args) throws IOException, InterruptedException {

		Runtime r = Runtime.getRuntime();
		
//		r.exec("notepad.exe");
		
		Process p = r.exec("notepad.exe");
		Thread.sleep(5000);//執行此進程5秒
		p.destroy();//殺掉子進程
	}
}
```
打印結果
```
//執行記事本應用約5秒
```
