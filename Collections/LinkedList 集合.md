# LinkedList 集合

## 常用方法

- 添加
addFirst();
addLast();

- 獲取
getFirst(); -> 獲取但不移除，如果鏈表為空，拋出 NoSuchElementException
getLast(); 
-> jdk 1.6 以後
peekFirst(); -> 獲取但不移除，如果鏈表為空，返回 null。
peekLast();

- 刪除
removeFirst(); -> 獲取並移除，如果鏈表為空，拋出 NoSuchElementException 
removeLast();
-> jdk 1.6 以後
pollFirst();> 獲取並移除，如果鏈表為空，返回 null。
pollLast();
##演示
```java
import java.util.Iterator;
import java.util.LinkedList;

public class LinkedListDemo {

	public static void main(String[] args) {
		
		LinkedList link = new LinkedList();
		
		link.addFirst("abc1");
		link.addFirst("abc2");
		link.addFirst("abc3");
		link.addFirst("abc4");
		
		System.out.println(link);

        //獲取一個元素便刪除一個元素
        while(!(link.isEmpty())) {
			
			System.out.println("remove: " + link.removeLast());
		}
		System.out.println(link);
		
		/*
		System.out.println("getFirst: " + link.getFirst());//獲取第一個元素且不刪除
		
		System.out.println("removeFirst: " + link.removeFirst());//刪除第一個元素
		
		System.out.println(link);
		*/
		
		/*
		Iterator it = list.iterator();
		while(it.hasNext()) {
			
			System.out.println(it.next());
		}
		*/
	}
}
```
打印結果
```
[abc4, abc3, abc2, abc1]
remove: abc1
remove: abc2
remove: abc3
remove: abc4
[]
```

## LinkedList 題目

請使用 LinkedList 來模擬一個堆棧或對列數據結構

- 1. 堆棧: 先進後出(First in Last Out) -> FILO
2. 對列: 先進先出(First in First Out) -> FIFO
- 我們應該描述這樣一個容器，給使用者提供一個容器完成這種結構中的一種。

```java
import java.util.LinkedList;

//隊列
public class LinkedListQueue {

	private LinkedList link;
	
	public LinkedListQueue() {
		
		link = new LinkedList();
	}
	
	
	//隊列的添加元素功能
	public void queueAdd(Object obj) {
		
		link.addLast(obj);
	}
	
	public Object queueRemove() {
		
		return link.removeFirst();
	}
	
	public boolean isNull() {
		
		return link.isEmpty();
	}
}

import java.util.LinkedList;

//堆棧
public class LinkedListStack {

	private LinkedList link;
	
	LinkedListStack() {
		
		link = new LinkedList();
	}
	
	public void stackAdd(Object obj) {
		
		link.addLast(obj);
	}
	
	public Object stackRemove() {
		
		return link.removeLast();
	}
	
	public boolean isNull() {
		
		return link.isEmpty();
	}
}

public class LinkedListTest {

	public static void main(String[] args) {
		
		LinkedListQueue q = new LinkedListQueue();
		
		q.queueAdd("abc1");
		q.queueAdd("abc2");
		q.queueAdd("abc3");
		
		while(!(q.isNull())) {
			
			System.out.println("Queue Remove: " + q.queueRemove());
		}
		
		System.out.println();
		
		LinkedListStack s = new LinkedListStack();
		
		s.stackAdd("abc1");
		s.stackAdd("abc2");
		s.stackAdd("abc3");
		
		while(!(s.isNull())) {
			
			System.out.println("Stack Remove: " + s.stackRemove());
		}
	}
}
```
打印結果
```
Queue Remove: abc1
Queue Remove: abc2
Queue Remove: abc3

Stack Remove: abc3
Stack Remove: abc2
Stack Remove: abc1
```
