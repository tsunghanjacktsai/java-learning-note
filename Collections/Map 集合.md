# Map 集合

## 作用
- 依次添加一對元素。Collection 一次添加一個元素。
- Map 也稱為雙列集合，Collection 集合稱為單列集合。
- Map 集合中存儲的是**鍵值對**。
- Map 集合中必須保證鍵的唯一性。

## 常用方法
- 添加
value put(key, value): 返回前一個和 key 關聯的值，如果沒有則返回 null。
- 刪除
void clear(): 清空 map 集合。
value remove(key): 根據指定的 key 刪除這個鍵值對。
- 判斷
boolean containsKey(key);
boolean containsValue(value); 
boolean isEmpty();
- 獲取
value get(key): 通過鍵獲取值，如果沒有該鍵返回 null，可以通過返回 null，來判斷是否包含指定鍵。
int size(); 獲取鍵值對的個數。
- **取出 Map 的所有元素:**
1. 原理: 通過 KeySet 方法獲取  Map 中所有的鍵所在的 Set 集合，在通過 Set 的迭代器獲取每一個鍵
  再對每一個鍵通過  Map 集合的 get 方法獲取其對應的值即可。
2. 通過 Map 轉成 Set 就可以迭代 -> **entrySet**
該方法將鍵和值的映射關係作為對象存儲到了 Set 集合中，
而這個映射關係的類型就是 Map, Entry 類型(結婚證)。

## Map 常用子類
- **HashTable**: 內部結構是哈希表，是同步的。不允許 null 作為鍵，null 作為值。
- **Properties:** 用來存儲鍵值隊形的配置文件信息，可以和 IO 技術相結合。
- **HashMap**: 內部結構是哈希表，不同步。允許 null 作為鍵，null 作為值。
- **TreeMap**: 內部結構是二插樹，不同步。可以對 Map 集合中的鏈進行排序。 

## 演示
### HashMap
```java
public class HashMapStudent {
	
	private String name;
	private int age;
	
	public HashMapStudent(String name, int age) {
		
		this.name = name;
		this.age = age;
	}
	
	public int hashCode() {
		
//		System.out.println(this + " @ hashcode");
		
		return name.hashCode() + age;
	}
	
	public boolean equals(Object obj) {
		
		//如果存取對象地址相同則直接返回真，不用進行多餘判斷
		if(this == obj) {
			
			return true;
		}
		
		//如果對象不屬於 HashMapPerson 類的話則直接返回假，不用進行多於判斷
		if(!(obj instanceof HashMapStudent)) {
			
			throw new ClassCastException("You put in the wrong class!");
		}
		
		System.out.println(this + " @ equals @" + obj);
		HashMapStudent p = (HashMapStudent) obj;
		
		return this.name.equals(p.name) && this.age == p.age;
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
	
	public String toString() {
		
		return name + " : " + age;
	}
}

public class HashMapDemo {

	public static void main(String[] args) {
		
		/*
		 * 將學生和學生的歸屬地通過鍵與值存儲到 Map 集合中
		 */
		
		HashMap<HashMapStudent, String> hm = new HashMap<HashMapStudent, String>();
		
		hm.put(new HashMapStudent("Jack", 10), "Taiwan");
		hm.put(new HashMapStudent("Vivian", 40), "Hong Kong");
		hm.put(new HashMapStudent("Lucas", 20), "German");
		hm.put(new HashMapStudent("Mike", 30), "America");
		hm.put(new HashMapStudent("Jack", 10), "Shanghai");
		
		Iterator<HashMapStudent> it = hm.keySet().iterator();
		
		while(it.hasNext()) {
			
			HashMapStudent key = it.next();
			String value = hm.get(key);
			System.out.println(key.getName() + " @ " + key.getAge() + " @ " + value);
		}
		
		//Map 的遍歷方式
		/*
		HashMap<String, String> map = new HashMap<>();
		map.put("aaa", "a");
		map.put("bbb", "b");
		map.put("ccc", "c");
		
		Iterator<String> it = map.keySet().iterator();
		
		while(it.hasNext()) {
			String key = it.next();
			String value = map.get(key);
			System.out.println(key + ", " + value);
		}
		
		Set<String> sets = map.keySet();
		for(String key : sets) {
			String value = map.get(key);
			System.out.println(key + ", " + value);
		}
		
		Set<Entry<String, String>> set1 = map.entrySet();
		for(Entry<String, String> entry : set1) {
			String key = entry.getKey();
			String value = entry.getValue();
			System.out.println(key + ", " + value);
		}
		*/
	}
}
```
打印結果
```
Jack : 10 @ equals @Jack : 10
Mike @ 30 @ America
Vivian @ 40 @ Hong Kong
Lucas @ 20 @ German
Jack @ 10 @ Shanghai
```

### TreeMap
```java
public class TreeMapStudent implements Comparable {
		
		private String name;
		private int age;
		
		public TreeMapStudent(String name, int age) {
			
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
		
		public String toString() {
			
			return name + " : " + age;
		}
		
		//比較名字和年齡進行比較以判斷要不要存入
		public int compareTo(Object o) {
			
			TreeMapStudent s = (TreeMapStudent) o;
			
//			int temp = this.age - s.age; 
//			
//			return temp == 0 ? this.name.compareTo(s.name) : temp;
			
			int temp = this.name.compareTo(s.name);
			
			return temp == 0 ? this.age - s.age : temp;
			
			
			/*
			if(this.age > s.age) {
				
				return 1;
			}
			
			if(this.age < s.age) {
				
				return -1;
			} else {
				
				return this.name.compareTo(s.name);
			}
			*/
		}
}

import java.util.Comparator;

public class TreeMapComparator implements Comparator<TreeMapStudent> {
		
		public int compare(TreeMapStudent o1, TreeMapStudent o2) {
			
			TreeMapStudent s1 = (TreeMapStudent) o1;
			TreeMapStudent s2 = (TreeMapStudent) o2;
			
			//重寫 compare 方法，更改比較法則
			int temp = s1.getName().compareTo(s2.getName());
			
			return temp == 0 ? s1.getAge() - s2.getAge() : temp;
//			return 1; //利用二叉樹判斷後面加入的對象均大於之前加入的對象
					  //形成有序排列
		}
}

import java.util.Iterator;
import java.util.Map;
import java.util.Set;
import java.util.TreeMap;

public class TreeMapDemo {

	public static void main(String[] args) {
		
		/*
		 * 將學生和學生的歸屬地通過鍵與值存儲到 Map 集合中
		 */
		
		TreeMap<TreeMapStudent, String> tm = new TreeMap<TreeMapStudent, String>(new TreeMapComparator());
		
		tm.put(new TreeMapStudent("Jack", 10), "Taiwan");
		tm.put(new TreeMapStudent("Vivian", 40), "Hong Kong");
		tm.put(new TreeMapStudent("Lucas", 20), "German");
		tm.put(new TreeMapStudent("Mike", 30), "America");
		tm.put(new TreeMapStudent("Jack", 10), "Shanghai");
		tm.put(new TreeMapStudent("Jervis", 50), "Suzhou");
		
		Iterator<Map.Entry<TreeMapStudent, String>> it = tm.entrySet().iterator();
		
		while(it.hasNext()) {
			
			Map.Entry<TreeMapStudent, String> me = it.next();
			TreeMapStudent key = me.getKey();
			String value = me.getValue();
			
			System.out.println(key.getName() + " @ " + key.getAge() + " @ " + value);
		}
	}
}
```
打印結果
```
Jack @ 10 @ Shanghai
Jervis @ 50 @ Suzhou
Lucas @ 20 @ German
Mike @ 30 @ America
Vivian @ 40 @ Hong Kong
```
