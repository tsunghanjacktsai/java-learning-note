# IOException

## 演示
```
import java.io.FileWriter;
import java.io.IOException;

public class IOExceptionDemo {
	
	private static final String LINE_SEPERATOR = System.getProperty("line.seperator");

	public static void main(String[] args) {
		
		FileWriter fw = null;
		
		try {
			
			fw = new FileWriter("demo.txt");
			
			fw.write("Hello" + LINE_SEPERATOR + "World");
			
		} catch(IOException e) {
			
			System.out.println(e.toString());
		} finally {
			
			if(fw != null) {	
				try {
					
					fw.close();
				} catch (IOException e) {
					
					throw new RuntimeException("Error to close");
				}
			}
		}
	}
}
```
