# List 接口

## List & Set 區別
- List: 有序(存入和取出的順序一致)，元素都有索引(角標)，元素可以重複。
  Set: 元素不能重複

## List 特有的常見方法
- 有一個共性特點就是都可以操作角標。
- 
1. 添加
void add(index, element);
void add(index, collection);

2. 刪除
Object remove(index);

3. 修改
Object set(index, element);

4. 獲取
Object get(index);
int indexOf(object);
int lastIndexOf(Object);
List subList(from, to);

- list 集合是可以完成對元素的增刪改查。

## ListIterator
- 作用: 實現在迭代過程中完成對元素的增刪改查。
- **注意:** 只有 List 集合具備該迭代功能。

## List常用對象
- Vector: 內部是數組數據結構，是同步的。(效率低，幾乎不用) -> **增刪，查詢都很慢** 
- ArrayList: 內部是數組數據結構，是不同步的。(提代了 vector) -> **查詢速度快**
- LinkedList: 內部是**鏈表**數據結構，是不同步的。-> **增刪元素的速度快**

## Example
```java
import java.util.ArrayList;
import java.util.List;

public class ListDemo {

	public static void main(String[] args) {

		List list = new ArrayList();
		
		show(list);
	}
	
	public static void show(List list) {
		
		//添加元素
		list.add("abc1");
		list.add("abc2");
		list.add("abc3");
		
		System.out.println(list);
		
		//插入元素
//		list.add(1, "abc5");
		
		//刪除元素
//		System.out.println("remove: " + list.remove(0));
		
		//修改元素
//		System.out.println("set: " + list.set(0, "Hello World"));
		
		//獲取元素
//		System.out.println("get: " + list.get(0));
		
		//獲取子列表
		System.out.println("sublist: " + list.subList(0, 2));
		
		System.out.println(list);
		
	}
}
```
打印結果
```
[abc1, abc2, abc3]
sublist: [abc1, abc2]
[abc1, abc2, abc3]
```
---
```
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;
import java.util.ListIterator;

public class ListDemo2 {

	public static void main(String[] args) {
		
		List list = new ArrayList();
		
		show(list);
	}
	
	public static void show(List list) {
		
		list.add("abc1");
		list.add("abc2");
		list.add("abc3");
		
		System.out.println(list);
		
		ListIterator it = list.listIterator();//獲取列表迭代器對象
		
		while(it.hasNext()) {
			
			Object obj = it.next();
			
			if(obj.equals("abc2")) {
				
//				it.set("Hello World");
				it.add("Hello World");
			}
			System.out.println("Next: " + obj);
		}
		System.out.println(list);
		
		
		
		while(it.hasPrevious()) {
			
			Object obj = it.previous();
			
			if(obj.equals("Hello World")) {
				
				it.set("abc5");
			}
			System.out.println("Previous: " + obj);
		}
		System.out.println(list);
		
/*		
		//List 特有的元素取出方式之一(Set 沒有) 
		for(int x = 0; x < list.size(); x++) {
			
			System.out.println("get: " + list.get(x));
		}
		
		
		
		Iterator it = list.iterator();
		
		while(it.hasNext()) {
			
			Object obj = it.next();//java.util.ConcurrentModificationException
								   //在迭代器過程中，不要使用集合操作元素，容易出現異常
								   //可以使用 Iterator 接口的子接口 ListIterator 來完成在迭代中隊元素進行更多操作
			
			if(obj.equals("abc2")) {
				
				list.add("abc5");
			} else {
			
				System.out.println("next: " + obj);
			}
		}
		System.out.println(list);
*/		
	}

}
```
打印結果
```
[abc1, abc2, abc3]
Next: abc1
Next: abc2
Next: abc3
[abc1, abc2, Hello World, abc3]
Previous: abc3
Previous: Hello World
Previous: abc2
Previous: abc1
[abc1, abc2, abc5, abc3]
```
