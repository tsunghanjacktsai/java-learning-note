# HashSet 集合

## Set 接口
- 元素**不重複**，是**無序**的
- Set 接口中的方法和 Collection 一致。

## HashSet
- 內部數據結構是哈希表，是不同步的。
- 哈希表確定元素是否相同
1. 判斷的是兩個元素的哈希值是否相同。
   如果相同，再判斷兩個對象的內是否相同。
2. 判斷哈希值相同，其實判斷的是對象的 hashCode 的方法。判斷內容相同，用的是 equals 方法。
**注意:** 如果哈希值不同，是不需要判斷 equals的 。
- HashSet 集合數據結構是哈希表，所以存儲元素的時候，
  使用的元素的 HashCode 方法來確定位置，如果位置相同，再通過元素的 equals 方法來確定是否相同。

## 演示
需求: 往 HashSet 集合中存儲 Person 對象。如果姓名年齡相同，視為同一個人，視為相同元素。
```
public class HashSetPerson {
	
	private String name;
	private int age;
	
	public HashSetPerson(String name, int age) {
		
		this.name = name;
		this.age = age;
	}
	
	public int hashCode() {
		
		System.out.println(this + " @ hashcode");
		
		return name.hashCode() + age;
	}
	
	public boolean equals(Object obj) {
		
		//如果存取對象地址相同則直接返回真，不用進行多餘判斷
		if(this == obj) {
			
			return true;
		}
		
		//如果對象不屬於 HashSetPerson 類的話則直接返回假，不用進行多於判斷
		if(!(obj instanceof HashSetPerson)) {
			
			throw new ClassCastException("You put in the wrong class!");
		}
		
		System.out.println(this + " @ equals @" + obj);
		HashSetPerson p = (HashSetPerson) obj;
		
		return this.name.equals(p.name) && this.age == p.age;
	}
	
	public void setName(String name) {
		
		this.name = name;
	}
	
	public String getName() {
		
		return name;
	}
	
	public void setAge() {
		
		this.age = age;
	}
	
	public int getAge() {
		
		return age;
	}
	
	public String toString() {
		
		return name + " : " + age;
	}
}

import java.util.HashSet;
import java.util.Iterator;

public class HashSetTest {

	public static void main(String[] args) {

		HashSet<HashSetPerson> hs = new HashSet<>();
		
		HashSetPerson p1 = new HashSetPerson("Jack", 20);
		HashSetPerson p2 = new HashSetPerson("Vivian", 18);
		HashSetPerson p3 = new HashSetPerson("Mike", 24);
		HashSetPerson p4 = new HashSetPerson("Jack", 20);
		
		hs.add(p1);
		hs.add(p2);
		hs.add(p3);
		hs.add(p4);
	
		System.out.println();
		
		Iterator it = hs.iterator();
		while(it.hasNext()) {
			
			HashSetPerson p = (HashSetPerson) (it.next());
			System.out.println(p.getName() + ", " + p.getAge());
		}
	}
}
```
打印結果
```
Jack : 20 @ hashcode
Vivian : 18 @ hashcode
Mike : 24 @ hashcode
Jack : 20 @ hashcode
Jack : 20 @ equals @Jack : 20

Jack, 20
Vivian, 18
Mike, 24    
```
