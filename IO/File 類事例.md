# File 類事例

## 需求
對指定目錄進行所有內容的列出(包含子目錄中的內容)，也可以理解為**深度遍歷**。

```java
import java.io.File;

public class FileTest {

	public static void main(String[] args) {

		File dir = new File("C:\\Users\\asus\\Documents\\Java\\JAVA SE Practice");
		
		listAll(dir, 0);
	}

    //遞歸算法
	public static void listAll(File dir, int level) {

		System.out.println(getSpace(level) + dir.getName());
		//獲取當前目錄下的文件夾或者文件對象
		
		level++;
		
		File[] files = dir.listFiles();
		
		for(int x = 0; x < files.length; x++) {
			
			if(files[x].isDirectory()) {

				listAll(files[x], level);
			} else {
			
			System.out.println(getSpace(level) + files[x].getAbsolutePath());
			}
		}
	}

	public static String getSpace(int level) {

		StringBuilder sb = new StringBuilder();
		
		for(int x = 0; x < level; x++) {
			
			sb.append("    ");
		}
		
		return sb.toString();
	}
}
```
打印結果
```
JAVA SE Practice
    C:\Users\asus\Documents\Java\JAVA SE Practice\.classpath
    C:\Users\asus\Documents\Java\JAVA SE Practice\.project
    .settings
        C:\Users\asus\Documents\Java\JAVA SE Practice\.settings\org.eclipse.core.resources.prefs
        C:\Users\asus\Documents\Java\JAVA SE Practice\.settings\org.eclipse.jdt.core.prefs
    C:\Users\asus\Documents\Java\JAVA SE Practice\A.txt
```

## 遞歸(複習)
- 一個功能在被重複使用，並每次使用時，參與運算的結果和上一次調用有關。這時可以用遞歸來解決問題。
- **注意:**
1. 遞歸一定要明確條件。否則容易棧溢出。
2. 注意一下遞歸的次數。

---

## 需求
刪除一個帶內容的目錄。

## 原理
必須要從最裡面往外刪。( Windows 進行刪除動作的規則)
需要進行深度遍歷。

## 演示
```java
import java.io.File;

public class RemoveDirTest {

	public static void main(String[] args) {

		File dir = new File("C:\\Users\\asus\\Documents\\New File");
		//dir.delete(); -> 不需要進行判斷條件的方法
		
		removeDir(dir);
	}

	public static void removeDir(File dir) {

		File[] files = dir.listFiles();
		
		for(File file : files) {
			
			if(file.isDirectory()) {
				
				removeDir(file);//遞歸
			} else {
				
				System.out.println(file + ": " + file.delete());
			}
		}
		
		System.out.println(dir + ": " + dir.delete());
	}
}
```
