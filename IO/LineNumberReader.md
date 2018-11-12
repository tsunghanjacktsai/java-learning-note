# LineNumberReader

## 演示
```
import java.io.FileReader;
import java.io.IOException;
import java.io.LineNumberReader;

public class LineNumberReaderDemo {

	public static void main(String[] args) throws IOException {

		FileReader fr = new FileReader("demo.txt");
		LineNumberReader lnr = new LineNumberReader(fr);
		
		String line = null;
		
		lnr.setLineNumber(100);//設定初始行為第100行
		
		while((line = lnr.readLine()) != null) {
			
			System.out.println(lnr.getLineNumber() + " @ " + line);
		}
		
		lnr.close();
	}
}
```
打印結果
```
101 @ Hello
102 @ World
103 @ Hello
104 @ World
```
