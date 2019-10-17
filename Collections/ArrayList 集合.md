# ArrayList 集合

## 演示
```java
public class ArrayListPerson {

	private String name;
	private int age;
	
	public ArrayListPerson(String name, int age) {
		
		this.name = name;
		this.age = age;
	}
	
	public void setName(String name) {
		
		this.name = name;
	}
	
	public String getName() {
		
		return name;
	}
	
	public void setAge(int age) {
		
		this.age = age;
	}
	
	public int getAge() {
		
		return age;
	}
}

import java.util.ArrayList;
import java.util.Iterator;

public class ArrayListDemo {

	public static void main(String[] args) {
		
		ArrayList<ArrayListPerson> al = new ArrayList<>();
		
		ArrayListPerson p1 = new ArrayListPerson("Jack", 20);
		ArrayListPerson p2 = new ArrayListPerson("Vivian", 20);
		ArrayListPerson p3 = new ArrayListPerson("Mike", 20);
		
		al.add(p1);
		al.add(p2);
		al.add(p3);
		
		Iterator it = al.iterator();
		while(it.hasNext()) {
			
			ArrayListPerson p = (ArrayListPerson) it.next();
			System.out.println(p.getName() + ", " + p.getAge());
		}
	}
}
```
打印結果
```
Jack, 20
Vivian, 20
Mike, 20
```

### 去除 ArrayList 中的重複元素
```java
public class ArrayListPerson {

	private String name;
	private int age;
	
	public ArrayListPerson(String name, int age) {
		
		this.name = name;
		this.age = age;
	}
	
	public boolean equals(Object obj) {
		
		//如果存取對象地址相同則直接返回真，不用進行多餘判斷
		if(this == obj) {
			
			return true;
		}
		
		//如果對象不屬於 HashSetPerson 類的話則直接返回假，不用進行多於判斷
		if(!(obj instanceof ArrayListPerson)) {
			
			throw new ClassCastException("You put in the wrong class!");
		}
		
		System.out.println(this + " @ equals @" + obj);
		
		ArrayListPerson p = (ArrayListPerson) obj;
		
		boolean b = this.name.equals(p.name) && this.age == p.age;
		
		System.out.println(b);
		
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

import java.util.ArrayList;
import java.util.Iterator;

public class ArrayListTest2 {

	public static void main(String[] args) {
		
		ArrayList al = new ArrayList();
		
//		al.add("abc1");
//		al.add("abc2");
//		al.add("abc3");
//		al.add("abc4");
//		
//		al.add("abc1");
//		al.add("abc2");
//		al.add("abc3");
		
		al.add(new ArrayListPerson("Jack1", 10));
		al.add(new ArrayListPerson("Jack2", 20));
		al.add(new ArrayListPerson("Jack3", 30));
		
		al.add(new ArrayListPerson("Jack1", 10));
		al.add(new ArrayListPerson("Jack2", 20));
		al.add(new ArrayListPerson("Jack3", 30));
		
		System.out.println(al);
		
		al = getSingleElement(al);
		
		System.out.println(al);
	}
	
	public static ArrayList getSingleElement(ArrayList al) {
		
		//1. 定義一個臨時容器
		ArrayList temp = new ArrayList();
		
		//2. 迭代 al 集合
		Iterator it = al.iterator();
		while(it.hasNext()) {
			
			//3. 判斷被迭代到的元素是否再臨時容器中存在
			Object obj = it.next();
			if(!(temp.contains(obj))) {
				
				temp.add(obj);
			}
		}
		return temp;
	}
}
```
打印結果
```
[Jack1 : 10, Jack2 : 20, Jack3 : 30, Jack1 : 10, Jack2 : 20, Jack3 : 30]
Jack2 : 20 @ equals @Jack1 : 10
false
Jack3 : 30 @ equals @Jack1 : 10
false
Jack3 : 30 @ equals @Jack2 : 20
false
Jack1 : 10 @ equals @Jack1 : 10
true
Jack2 : 20 @ equals @Jack1 : 10
false
Jack2 : 20 @ equals @Jack2 : 20
true
Jack3 : 30 @ equals @Jack1 : 10
false
Jack3 : 30 @ equals @Jack2 : 20
false
Jack3 : 30 @ equals @Jack3 : 30
true
[Jack1 : 10, Jack2 : 20, Jack3 : 30]

```
