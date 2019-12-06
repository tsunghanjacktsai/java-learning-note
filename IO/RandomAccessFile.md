# RandomAccessFile

## 特點
1. 該對象既既能讀也能寫。
2. 該對象內部維護了一個 byte 數組，並通過指針可以操作數組中的元素。
3. 可以通過 getFilePointer 方法獲取指針的位置，和通過 seek 方法設置。
4. 其實該對象就是將**字節輸入流和輸出流進行封裝。**
5. 該對象的源或者目的只能是文件。通過**構造函數**可以看出。

## 演示
```java
import java.io.IOException;
import java.io.RandomAccessFile;

public class RandomAccesFileDemo {

	public static void main(String[] args) throws IOException {

//		writeFile();
//		readFile();
		randomWrite();
	}
	
	public static void randomWrite() throws IOException {

		RandomAccessFile raf = new RandomAccessFile("RanAccFile.txt", "rw");
		
		//往指定位置寫入數據
		raf.seek(3 * 8);
		
		raf.write("Vivi".getBytes());
		raf.writeInt(102);
		
		raf.close();
	}

	public static void readFile() throws IOException {

		RandomAccessFile raf = new RandomAccessFile("RanAccFile.txt", "r");
		
		//通過 seek 設置指針位置
		raf.seek(1 * 8);//隨機讀取，只要指定指針位置即可
		
		byte[] buf = new byte[4];
		raf.read(buf);
		
		String name = new String(buf);
		
		int age = raf.readInt();
		
		System.out.println("name = " + name);
		System.out.println("age = " + age);
		
		System.out.println("pos: " + raf.getFilePointer());
		
		raf.close();
	}

	//使用 RandomAccessFile 對象寫入信息，比如姓名和年齡
	public static void writeFile() throws IOException {
		/*
		 * 如果文件存在，不創建，如果存在，則創建
		 */
		
		RandomAccessFile raf = new RandomAccessFile("RanAccFile.txt", "rw");
		
		raf.write("Jack".getBytes());
		raf.writeInt(97);
		raf.write("Mike".getBytes());
		raf.writeInt(99);
		
		raf.close();
	}
}
```
