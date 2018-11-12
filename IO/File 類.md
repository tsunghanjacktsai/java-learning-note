# File 類

## 特點
- 用來將已存在或不存在的文件或者文件夾封裝成對象。
- 方便對文件與文件夾的屬性信息進行操作。
- File 對象可以作為參數傳遞給流的構造函數。

## File 對象的常見方法
- 獲取
1. 獲取文件名稱
2. 獲取文件路徑
3. 獲取文件大小
4. 獲取文件修改時間

- 創建和刪除
- 判斷
- 重命名
- 系統根目錄及容量獲取
- 獲取目錄內容
 - 獲取當前目錄下的文件以及文件夾的名稱，包含隱藏文件。
   調用 list 方法的 File 對象中封裝的必須是目錄。
   否則會發生 **NullPointerException**。
   如果訪問的是**系統級目錄**也會發生**空指針異常**。
 - 如果目錄存在但是沒有內容，會返回一個數組，但長度為 0。

## 演示
```
import java.io.File;
import java.io.IOException;
import java.text.DateFormat;
import java.util.Date;

public class FileMethodDemo {

	public static void main(String[] args) throws IOException {

//		getDemo();
		
//		createDeleteDemo();
		
//		isDemo();
		
//		renameDemo();
		
//		listRootsDemo();
		
		listDemo();
	}
	
	public static void getDemo() {
	
		File file = new File("a.txt");

		String name = file.getName();
		
		String absPath = file.getAbsolutePath();//絕對路徑(開頭有盤符即為絕對)
		
		String path = file.getPath();//相對路徑(寫什麼拿甚麼)
		
		long len = file.length();//剩餘空間
		
		long time = file.lastModified();//返回此抽象路径名表示的文件上次修改的时间
		Date date = new Date(time);
		DateFormat dateFormat = DateFormat.getDateTimeInstance(DateFormat.LONG, DateFormat.LONG);
		String str_time = dateFormat.format(date);
		
		
		System.out.println("name @ " + name);
		System.out.println("absPath @ " + absPath);
		System.out.println("path @ " + path);
		System.out.println("len @ " + len);
		System.out.println("time @ " + str_time);
	}
	
	public static void createDeleteDemo() throws IOException {
		
		File dir = new File("abc");
		
		boolean b = dir.mkdir();//make directory -> 創建名為 abc 的目錄(文件夾)
		System.out.println("b = " + b);
		
		boolean d = dir.delete();
		System.out.println("d = " + d);
		
		//文件的創作和刪除
//		File file = new File("file.txt");
		
		/*
		 * 和輸出流不一樣，如果文件不存在，則創建，如果存在就不創建
		 */
		
//		boolean b = file.createNewFile();//創建
//		
//		System.out.println("b = " + b);
		
//		boolean b = file.delete();//刪除
//		
//		System.out.println("b = " + b);
	}
	
	public static void isDemo() {
		
		File file = new File("a.txt");
		
		System.out.println(file.exists());//判斷文件是否存在(優先)
		
		System.out.println(file.isFile());//判斷是否是文件
		System.out.println(file.isDirectory());//判斷是否是目錄
	}
	
	public static void renameDemo() {
		
		File f1 = new File("c:\\aa.txt");
		
		File f2 = new File("c:\\bb.txt");
		
		boolean b = f1.renameTo(f2);
		
		System.out.println(b);
	}
	
	//系統根目錄及容量獲取
	public static void listRootsDemo() {
		
//		File[] files = File.listRoots();
//		
//		for(File file : files) {
//			System.out.println(file);
//		}
		
		File file = new File("d:\\");
		
		System.out.println("getFreeSpace: " + file.getFreeSpace());
		System.out.println("getTotalSpace: " + file.getTotalSpace());
		System.out.println("getUsableSpace: " + file.getUsableSpace());
	}
	
	//獲取目錄內容
	public static void listDemo() {
		
		File file = new File("c:\\");
		
		String[] names = file.list(new SuffixFilter(".java"));//創建過濾器以過濾出目錄下指定的文件類型
		
		for(String name : names) {
			System.out.println(names);
		}
	}
}

import java.io.File;
import java.io.FilenameFilter;

//過濾器
public class SuffixFilter implements FilenameFilter {
	
	private String suffix;
	
	public SuffixFilter(String suffix) {
		
		super();
		this.suffix = suffix;
	}

	@Override
	public boolean accept(File dir, String name) {
		
		return name.endsWith(suffix);
	}
}
```
 
