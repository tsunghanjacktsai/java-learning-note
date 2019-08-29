# Properties 集合事例

## 需求
定義功能，獲取一個應用程序運行的次數，如果超過 5 次，給出使用 "次數已到，請註冊" 的提示。

## 思路
1. 有計數器，每次程序啟動都需要技術一次，
   並且是在原有的次數上進行記數。
2. 計數器就是一個變量。程序啟動時進行記數，計數器必須存在於內存並進行運算。
   一旦程序結束，計數器消失。再次啟動該程序，計數器又再度被初始化。
   而我們需要多次啟動同一個應用程序，使用的是同一個記數器，這就需要記數器的生命週期變長，從內存存儲到硬盤文件中。
3. 如何使用記數器?
   首先，程序啟動時，應該先讀取這個用於紀錄記數器信息的配置文件。獲取上一次記數器次數。並進行試用次數的判斷。
   其次，對該次數進行自增，並自增後的次數重新存儲到配置文件中。00
4. 文件中的信息該如何進行存儲並體現?
   直接存儲次數值可以，但是不明確該數據的涵義，所以起名字很重要。這就有了名字和值的對應，所以可以使用鍵值對。
   可是映射關係由 map 集合搞定，又需要讀取硬盤上的數據，所以 map + io = Properties.

```
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.Properties;

public class PropertiesTest {

	public static void main(String[] args) throws IOException {

		getAppCount();
	}
	
	//計數器
	public static void getAppCount() throws IOException {
		
		//將配置文件封裝成 File 對象
		File confile = new File("logging.properties");
		
		if(!confile.exists()) {
			
			confile.createNewFile();
		}
		
		FileInputStream fis = new FileInputStream(confile);
		
		Properties prop = new Properties();
		
		prop.load(fis);
		
		//從集合中通過鍵獲取字數
		String value = prop.getProperty("time");
		
		int count = 0; //定義計數器
		if(value != null) {
			
			count = Integer.parseInt(value);
			if(count >= 5) {
				
//				System.out.println("使用次數已到，請重新註冊");
//				return;
				throw new RuntimeException("使用次數已到，請重新註冊");
			}
		}
		count++;
		
		//將改變後的次數重新存儲到集合中
		prop.setProperty("time", count + "");
		
		FileOutputStream fos = new FileOutputStream(confile);
		
		prop.store(fos, "");
		
		fos.close();
		fis.close();
	}
}
```
