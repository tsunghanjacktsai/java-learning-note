# DataStream

## 使用
操作基本數據類型的流對象。

## 演示
```java
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class DataStreamDemo {

	public static void main(String[] args) throws IOException {

		writeData();
		readData();
	}
	
	public static void readData() throws IOException {

		DataInputStream dis = new DataInputStream(new FileInputStream("data.txt"));
		
		String str = dis.readUTF();
		
		System.out.println(str);
	}

	public static void writeData() throws IOException {
		
		DataOutputStream dos = new DataOutputStream(new FileOutputStream("data.txt"));
		
		dos.writeUTF("你好");
		
		dos.close();
	}

}
```
打印結果
```
你好
```
