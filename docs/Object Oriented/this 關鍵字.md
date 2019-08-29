# this 關鍵字

## 重點
- 使用時機: 當成員變量和局部變量重名，可以用關鍵字this來區分。
- this: 代表當前對象 -> 所在函數所屬對象的引用，哪個對象調用了this所在的函數，this就代表哪個對象。
- 一個類中的成員(變量 or 方法)想要被執行，就必須要有對象調用。
- this 也可以用於在構造函數中調用其他構造函數。
注意: 只能定義在構造函數中的第一行。因為初始化動作要先執行。

## Example

```java
public class PersonDemo {
		
	public static void main(String[] args) {
		
		Person p = new Person("Mike", 20);
		Person p1 = new Person("Jack", 19);
		p.talk();
		System.out.println(p.compare(p1));
	}
}

public class Person {
	
	private int age;
	private String name;
	
	//定義一個 Person 類的構造函數
	Person(String name){
	    this.name = name;
	}
	Person(String n, int a){
	    this(name);//如果想調用前面一個構造函數，可使用此形式調用。
		this.name = n;
		this.age = a;
	}
	
	public void setName(String n) {//仍要設置getter與setter，以免需求改動時難以改變參數值
		this.name = n;
	}
	
	public String getName() {
		
		return name;
	}
	
	public void talk() {
		System.out.println(this.name + ":" + this.age);
	}
	
	//比較年齡
	public boolean compare(Person p) {
		
		if(this.age == p.age) {
			return true;
		}else {
			return false;
		}
	}
}
```
