# Object 類

## 定義
- 所有類的根類。
- Object 是不斷抽取而來，具備著**所有對象都具備的共性內容**。

## equals()
- 格式: a.equals(b);
- 一般都會重寫equals方法，根據對象的特有內容，建立判斷對象是否相同的依據。

## hashCode()
- 格式: a.hashCode();
- 返回該對象的哈希值，此方法是為了提高哈希表的功能。
- **注意:** 當此方法被重寫時，通常有必要重寫 hashCode 方法。-> 相等對象必須有相等的哈希值。

## getClass()
- 格式: a.getClass();
- 返回此對象的運行類。

## toString()
- 格式: a.toString(); -> Public String toString() {}
- 返回該字符串的表現形式。
- 一般直接重寫此方法。

## Example
```
public class Person {

	private int num;
	
	public Person(int num) {
		
		this.num = num;
	}
	
	//重寫 Object 方法
	public boolean equals(Object obj) { //Object obj = b -> 向上轉型
		
		if(!(obj instanceof Person)) {
		
			throw new ClassCastException("Wrong Class Cast");
		}
		Person p = (Person) obj;
		return this.num == p.num;		
	}
}

public class Person2 {

}

public class Demo {

	public static void main(String[] args) {
		
		Person a = new Person(20);
		Person b = new Person(20);
		
		System.out.println(a.equals(b)); //equals 方法
		
		
		System.out.println(Integer.toHexString(a.hashCode())); //返回哈希值(十進制)
		
		//getClass -> 返回對象的運行類
		Class c1 = a.getClass();
		Class c2 = b.getClass();
		
		System.out.println(c1 == c2);
		System.out.println(c1.getName()); //返回運行類名
		
		System.out.println(a); //等於  getClass().getName() + "@" + Integer.toHexString(a.hashCode());
	}
}
```
### 打印結果
```
true
2401f4c3
true
OOP.ObjectPerson
OOP.ObjectPerson@2401f4c3
```
