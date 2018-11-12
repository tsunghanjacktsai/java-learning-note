# TreeSet 集合

## 作用
- 可以對 set 集合中的元素進行排序。
- 判斷元素唯一性的方式，就是根據比較方法的返回結果返回是否是0，是 0 及為相同元素，不存入。
- TreeSet 對元素進行排序的方式:
1. 讓元素自身具備比較功能，元素就須要實現 Comparable 接口，並重寫 compareTo 方法。

- 如果不要按照對象中具備的自然排序進行排序。如果對象中不具備自然順序。
  可以使用 TreeSet 集合的第二種排序方式
1. 讓集合自身具備比較功能，定義一個類實現 Comparator 接口，重寫 compare 方法。
   將該類對象作為參數傳遞給 TreeSet 集合的構造函數。

## 演示
```
public class TreeSetPerson implements Comparable {
	
	private String name;
	private int age;
	
	public TreeSetPerson(String name, int age) {
		
		this.name = name;
		this.age = age;
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
	
	//比較名字和年齡進行比較以判斷要不要存入
	public int compareTo(Object o) {
		
		TreeSetPerson p = (TreeSetPerson) o;
		
//		int temp = this.age - p.age; 
//		
//		return temp == 0 ? this.name.compareTo(p.name) : temp;
		
		int temp = this.name.compareTo(p.name);
		
		return temp == 0 ? this.age - p.age : temp;
		
		
		/*
		if(this.age > p.age) {
			
			return 1;
		}
		
		if(this.age < p.age) {
			
			return -1;
		} else {
			
			return this.name.compareTo(p.name);
		}
		*/
	}
}

import java.util.Comparator;

//創建一個根據 TreeSetPerson 類的 name 進行排序的比較器
public class TreeSetComparator implements Comparator {
	
	public int compare(Object o1, Object o2) {
		
		TreeSetPerson p1 = (TreeSetPerson) o1;
		TreeSetPerson p2 = (TreeSetPerson) o2;
		
		//重寫 compare 方法，更改比較法則
		int temp = p1.getName().compareTo(p2.getName());
		
		return temp == 0 ? p1.getAge() - p2.getAge() : temp;
//		return 1; //利用二叉樹判斷後面加入的對象均大於之前加入的對象
				  //形成有序排列
	}
}

import java.util.Iterator;
import java.util.TreeSet;

public class TreeSetDemo {

	public static void main(String[] args) {

		TreeSet ts = new TreeSet(new TreeSetComparator());
		
		ts.add(new TreeSetPerson("Jack", 20));
		ts.add(new TreeSetPerson("Vivian", 10));
		ts.add(new TreeSetPerson("Jervis", 50));
		ts.add(new TreeSetPerson("Mike", 40));
		ts.add(new TreeSetPerson("Jacky", 50));
		
		Iterator it = ts.iterator();
		while(it.hasNext()) {
			
			TreeSetPerson p = (TreeSetPerson) it.next();
			
			System.out.println(p.getName() + ", " + p.getAge());
		}
	}
}
```
打印結果
```
Jack, 20
Jacky, 50
Jervis, 50
Mike, 40
Vivian, 10
```

對字符串進行長度排序
```
import java.util.Iterator;
import java.util.TreeSet;

public class TreeSetTest {

	public static void main(String[] args) {

		TreeSet ts = new TreeSet(new TreeSetTestComparator());
		
		ts.add("aaaaa");
		ts.add("zz");
		ts.add("nbaq");
		ts.add("cba");
		ts.add("abc");
		
		Iterator it = ts.iterator();
		
		while(it.hasNext()) {
			
			System.out.println(it.next());
		}
	}
}
```
打印結果
```
zz
abc
cba
nbaq
aaaaa
```
