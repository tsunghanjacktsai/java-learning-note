# Utilities 工具類

## Collections
是集合框架的工具類，裡面的方法都是**靜態**的。

## 演示
```java
import java.util.Comparator;

public class ToolComparator implements Comparator<String> {

	@Override
	public int compare(String o1, String o2) {

		int temp = o1.length() - o2.length();
		
		return temp == 0 ? o1.compareTo(o2) : temp;
	}
}

import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;
import java.util.TreeSet;

public class ToolCollectionsDemo {

	public static void main(String[] args) {
		
		demo_1();
		System.out.println();
		
		demo_2();
		System.out.println();
		
		demo_3();
		System.out.println();
		
		demo_4();
		System.out.println();
	}
	
	//排序
	public static void demo_1() {
		
		List<String> list = new ArrayList<>();
		
		list.add("abcde");
		list.add("cde");
		list.add("ade");
		list.add("betrue");
		list.add("zzz");
		
		System.out.println(list);
		
		//對 list 集合進行指定排序
		Collections.sort(list);
		System.out.println(list);
		
		mySort(list);
		System.out.println(list);	
		
		//對 list 集合長度進行排序
		mySort(list, new ToolComparator());
		System.out.println(list);
		
		Collections.sort(list, new ToolComparator());
		System.out.println(list);
	}
	
	//折半查找
	public static void demo_2() {
		
		List<String> list = new ArrayList<>();
		
		list.add("abcde");
		list.add("cde");
		list.add("ade");
		list.add("betrue");
		list.add("zzz");
		
		Collections.sort(list);
		System.out.println(list);
		
		int index = Collections.binarySearch(list, "cde");
		
		System.out.println("Index = " + index);
		
		//獲取最大值
		String max = Collections.max(list, new ToolComparator());//獲取最大長度
		System.out.println("Max = " + max);
	}

	//反轉排序
	public static void demo_3() {
		/*
		TreeSet<String> ts = new TreeSet<String>(new Comparator<String>() {
			
			public int compare(String o1, String o2) {
				
				int temp = o2.compareTo(o1);
				return temp;
			}
		});
		*/
		
//		TreeSet<String> ts = new TreeSet<>(Collections.reverseOrder());//反轉按存儲順序排序
		TreeSet<String> ts = new TreeSet<>(Collections.reverseOrder(new ToolComparator()));//反轉按長度排序
		
		ts.add("abc");
		ts.add("hahahahah");
		ts.add("cdefg");
		ts.add("styvuide");
		ts.add("cba");
		
		System.out.println(ts);
	}
	
	//替換
	public static void demo_4() {
		
		List<String> list = new ArrayList<String>();
		
		list.add("abcde");
		list.add("cba");
		list.add("aa");
		list.add("cba");
		
		System.out.println(list);
		
		Collections.replaceAll(list, "cba", "nba");//set(indexOf("cba"), "nba");
		System.out.println(list);
	}
	
	public static <T extends Comparable<? super T>> void mySort(List<T> list) {
		
		for(int i = 0; i < list.size() - 1; i++) {
			
			for(int j = i + 1; j < list.size(); j++) {
				/*
				T temp = list.get(i);
				list.set(i, list.get(j));
				list.set(j, temp);
				*/
				Collections.swap(list, i, j);
			}
		}
	}
	
	public static <T> void mySort(List<T> list, Comparator<? super T> comp) {
		
		for(int i = 0; i < list.size() - 1; i++) {
			
			for(int j = i + 1; j < list.size(); j++) {
				
				if(comp.compare(list.get(i), list.get(j)) > 0) {
					
					Collections.swap(list, i, j);
				}
				
				/*
				T temp = list.get(i);
				list.set(i, list.get(j));
				list.set(j, temp);
				*/
//				Collections.swap(list, i, j);
			}
		}
	}
}
```
打印結果
```
[abcde, cde, ade, betrue, zzz]
[abcde, ade, betrue, cde, zzz]
[zzz, cde, betrue, ade, abcde]
[ade, cde, zzz, abcde, betrue]
[ade, cde, zzz, abcde, betrue]

[abcde, ade, betrue, cde, zzz]
Index = 3
Max = betrue

[hahahahah, styvuide, cdefg, cba, abc]

[abcde, cba, aa, cba]
[abcde, nba, aa, nba]
```

## Arrays

### **List asList** 
- 將數組轉成集合。
  **好處:** 其實可以使用集合的方法操作數組中的元素。
  **注意:** 數組的長度是固定的，所以對於集合的增刪方法是不可以使用的，否則發生**UnsupportedOperationException**。
- 如果數組中的元素是**對象**，那麼轉成集合時，直接將數組中的元素作為集合中的元素進行存儲。
  如果數組中的元素是**基本類型數值**，那麼會將該數組作為集合中的元素進行存儲。

### **ToArray**
- 集合轉數組
- 使用的是 Collection 接口中的 **toArray** 方法。
- 可以對集合中的元素操作的方法進行限定。不允許對其進行增刪。
- toArray 方法需要傳入一個指定類型的數組。
  如果長度小於集合的 size，那麼該方法會創建一個同類型並和集合中相同 size 的數組。
  如果長度大於集合的 size，那麼該方法就會使用指定的數組，存儲集合中的元素，其他位置莫認為 **null**。
  所以建議，最後長度就指定為**集合的 size**。

### **List asList**
```java
import java.util.Arrays;
import java.util.List;

public class ArraysDemo {

	public static void main(String[] args) {
		
		int[] arr = {1, 3, 5, 7, 9};
		String[] arr_2 = {"abc", "def", "hij"};
		
		System.out.println(Arrays.toString(arr));
		System.out.println(myToString(arr));
		
		List<String> list = Arrays.asList(arr_2);//判斷是否包含
		boolean b = list.contains("abc");
		System.out.println("list contains: " + b);
		
		demo_2();
		
//		list.add("Hello World");
//		System.out.println(list);//UnsupportedOperationException -> 數組的長度是固定的
		
	}
	
	//數組轉集合
	public static void demo_2() {

		int[] arr = {31, 11, 51, 61};
		
		List<int[]> list = Arrays.asList(arr);//泛型是數組(對象)
		
		System.out.println(list);
	}

	public static boolean myContains(String[] arr, String key) {
		
		for(int i = 0; i < arr.length; i++) {
			
			if(arr[i].equals(key))
				return true;
		}
		return false;
	}
	
	//toString 的經典實現
	public static String myToString(int[] a) {
		
		int iMax = a.length - 1;
		if(iMax == -1)
			return "[]";
		
		StringBuilder b = new StringBuilder();
		b.append('[');
		for(int i = 0; ; i++) {//中間去掉條件判斷句以提高效率
			b.append(a[i]);
			if(i == iMax) {
				return b.append(']').toString();
			}
			b.append(", ");
		}
	}
}
```
打印結果
```
[1, 3, 5, 7, 9]
[1, 3, 5, 7, 9]
list contains: true
[[I@3d4eac69] //對象
```
---
### **toArray**
```java
import java.util.ArrayList;
import java.util.Arrays;

public class CollectionsToArrayDemo {

	public static void main(String[] args) {
		
		ArrayList<String> list = new ArrayList<>();
		
		list.add("abc1");
		list.add("abc2");
		list.add("abc3");
		list.add("abc4");
		
		String[] arr = list.toArray(new String[list.size()]);
		
		System.out.println(Arrays.toString(arr));
		
	}
}
```
打印結果
```
[abc1, abc2, abc3, abc4]
```
