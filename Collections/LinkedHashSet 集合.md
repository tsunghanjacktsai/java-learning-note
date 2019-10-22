# LinkedHashSet 集合

## 作用
形成有序的 HashSet 集合。

## 演示
```java
import java.util.HashSet;
import java.util.Iterator;
import java.util.LinkedHashSet;

public class LinkedHashSetDemo {

	public static void main(String[] args) {
		
//		HashSet hs = new HashSet();
//		
//		hs.add("Hello World --- 1");
//		hs.add("You are beautiful --- 2");
//		hs.add("I'm so handsome --- 3");
//		hs.add("I love you --- 4");
		
		LinkedHashSet lhs = new LinkedHashSet();
		
		lhs.add("Hello World --- 1");
		lhs.add("You are beautiful --- 2");
		lhs.add("I'm so handsome --- 3");
		lhs.add("I love you --- 4");
		
		
		/*
		不能重複，複製再多都一樣
		hs.add("I'm so handsome");
		hs.add("I'm so handsome");
		hs.add("I'm so handsome");
		hs.add("I'm so handsome");
		*/
		
		Iterator it = lhs.iterator();
		while(it.hasNext()) {
			
			System.out.println(it.next());
		}
	}
}
```
打印結果
```
Hello World --- 1
You are beautiful --- 2
I'm so handsome --- 3
I love you --- 4
```
