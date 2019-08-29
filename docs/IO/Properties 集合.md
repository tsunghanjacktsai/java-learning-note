# Properties 集合

## 特點
- 該集合中的鍵和值都是字符串類型。
- 集合中的數據可以保存到流中，或者從流獲取。
- 通常該集合用於操作以鍵值對形式存在的配置文件。

## 演示
```
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.util.Properties;
import java.util.Set;

public class PropertiesDemo {

	public static void main(String[] args) throws IOException {

//		propertiesDemo();
		
//		propertiesDemo_2();
		
//		propertiesDemo_3();
		
//		propertiesDemo_4();
		
		test();
	}
	
	//Properties 集合的存和取
	public static void propertiesDemo() {
		
		//創建一個 Properties 集合
		Properties prop = new Properties();
		
		//存儲元素
		prop.setProperty("Jack", "20");
		prop.setProperty("Vivian", "30");
		prop.setProperty("Jervis", "10");
		prop.setProperty("Mike", "40");
		
		//修改元素
		prop.setProperty("Jervis", "50");
		
		//取出所有元素
		Set<String> names = prop.stringPropertyNames();
		
		for(String name : names) {
			
			String value = prop.getProperty(name);
			
			System.out.println(name + " @ " + value);
		}
	}
	
	//演示 Properties 和流對象相結合的功能
	public static void propertiesDemo_2() {
		
		//創建一個 Properties 集合
		Properties prop = new Properties();
				
		//存儲元素
		prop.setProperty("Jack", "20");
		prop.setProperty("Vivian", "30");
		prop.setProperty("Jervis", "10");
		prop.setProperty("Mike", "40");
		
		prop.list(System.out);
	}
	
	//store 方法
	public static void propertiesDemo_3() throws IOException {
		
		//創建一個 Properties 集合
		Properties prop = new Properties();
						
		//存儲元素
		prop.setProperty("Jack", "20");
		prop.setProperty("Vivian", "30");
		prop.setProperty("Jervis", "10");
		prop.setProperty("Mike", "40");
		
		/*
		 * 想將這些集合中的字符串鍵值信息持久化存儲到文件中
		 * 需要關聯輸出流
		 */
		FileOutputStream fos = new FileOutputStream("info.txt");
		
		//將集合數據存儲到文件中，使用 store 方法
		prop.store(fos, "name + age");
		
		fos.close();
	}
	
	//load 方法
	public static void propertiesDemo_4() throws IOException {
		
		Properties prop = new Properties();
		
		//集合中的數據來自於一個文件
		//注意: 必須要保證該文件中的數據是鍵值對
		//需要使用到讀取流
		FileInputStream fis = new FileInputStream("info.txt");
		
		//使用 load 方法
		prop.load(fis);
		
		/*
		 * 等於 load 方法
		 * BufferedReader bufr = new BufferedReader(new FileReader("intfo.txt"));
		 * 
		 * String line = null;
		 * 
		 * while((line = bufr.readLine()) != null) {
		 * 
		 * 		if(line.startsWith("#"))
		 * 			continue;
		 * 
		 * 		String[] arr = line.split("=");
		 *
		 * 		prop.setProperty(arr[0], arr[1]);
		 * }
		 * 
		 * prop.list(System.out);
		 * 
		 * bufr.close();
		 */
		
		prop.list(System.out);
	}
	
	//對已有的配置文件進行修改
	/*
	 * 讀取這個文件，
	 * 並將這個文件中的鍵值數據存儲到集合中，
	 * 在通過集合將進行修改，
	 * 在通過流將修改後的數據存儲到文件中
	 */
	public static void test() throws IOException {
		
		//讀取這個文件
		File file = new File("info.txt");
		if(!file.exists()) {
			file.createNewFile();
		}
		FileReader fr = new FileReader("info.txt");
		
		//創建集合存儲配置信息
		Properties prop = new Properties();
		
		//將流中信息存儲到集合中
		prop.load(fr);
		
		//修改鍵值數據
		prop.setProperty("Jervis", "50");
		
		FileWriter fw = new FileWriter(file);
		
		prop.store(fw, "Name + Age");
		
//		prop.list(System.out);
		
		fw.close();
		fr.close();
	}
}
```
