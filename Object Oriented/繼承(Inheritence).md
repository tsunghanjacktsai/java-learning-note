# 繼承  (Inheritence)

## 繼承的好處
1. 提高代碼復用性。
2. 讓類與類之間產生了關係，給另一個特徵多態提供了前提。

## Java繼承機制
- Java中支持單繼承，不直接支持多繼承，但對 C++ 中的多繼承機制進行改良。
- 單繼承: 一個子類只能有一個直接父類。
- 多繼承: 一個子類可以有多個直接父類
          不直接支持，因為多個父類中有相同成員，會產生調用的不確定性。
          在 Java 中是通過"多實現"的方式來體現。
- Java 支持多重(多層)繼承 -> C繼承B, B繼承A -> 出現繼承體系
- 當要使用一個繼承體系時，
1. 查看該體系中的頂層類，了解該體系的基本功能。
2. 創建體系中的最子類對象，完成功能的使用。

## 使用時機
- 當類與類之間存在著所屬關係的時候，就定義繼承。(所屬關係 -> is a 關係)
  判斷被繼承的類的所有功能是否是子類都應該具備的。
- Example: x是y的一種 -> x extends y

## 子父類中成員特點
- **成員變量**
  當本類的成員和局部變量同名用this區分。
  當子父類中的成員變量同名用super區分父類。
  this: 代表一個本類對象的引用。
  super: 代表一個父類的空間。
**注意:   父類成員變量用 private，則子類不可對其進行直接訪問，僅能透過 getter, setter 對其進行間接訪問。**

- **成員函數**
  當子父類中出現成員函數一模一樣的情況，會優先運行子類的函數。此稱為 Override (重寫 or 覆蓋)。
  **函數的兩個特性**
1. Overload (重載): 同一個類中
2. Override (重寫、覆蓋): 子類中

  **Override 注意事項**
1. 子類權限必須要大於等於父類的權限
   -> 父類用 public, 則子類不能用默認權限或其他低於 public 的權限。
2. 靜態只能重寫靜態，或被靜態重寫。

  **Override 使用時機**
  當對一個類進行子類的擴展時，子類需要保留父類的功能聲明，
  但是要定義子類中該功能的特有內容時，就使用 Override 來完成。

- **構造函數**
1. **特點:** 在子類的構造函數中第一行有一個默認的隱藏語句 -> super();

2. **子類的實例化過程:** 子類中所有的構造函數默認都會訪問父類中空參數的構造函數。
3. **為什麼子類實例化的時候要訪問父類中的構造函數?**
因為子類繼承了父類，獲取到了父類中內容(屬性)，所以在使用父類內容之前，要先看父類是如何對自己的內容進行初始化的，
子類在構造對象時，必須訪問父類中的構造函數。
為了完成這個必需的動作，就在子類的構造函數中加入了 super() 語句。
如果父類中沒有定義空參數構造函數，那麼子類的構造函數必須用 super 明確要調用父類中哪個構造函數。
同時子類構造函數中如果使用 this 調用了本類構造函數時，那麼 super 就消失了，因為 super 和 this 都只能定義在第一行，所以只能有一個。
但是可以保證的是，子類中肯定會有其他的構造函數訪問父類的構造函數。
**注意: super語句必須要定義在子類構造函數的第一行。因為父類的初始化動作要先完成。**

## 對象實例化過程
Person p = new Person();
1. JVM 會讀取指定的路徑下的 Person.class 文件，並加載進內存。
   並會先加載 Person 的父類(如果有直接的父類的情況下)。
2. 在對內存中開闢空間，分配地址。
3. 在對象空間中，對對象中的屬性進行默認初始化。
4. 調用對應的構造函數進行初始化。
5. 在構造函數中，第一行會先調用父類中的構造函數進行初始化。
6. 父類初始化完畢後，再對子類的屬性進行顯示初始化。
7. 再進行子類構造函數的特定初始化。
8. 初始化完畢後，將地址值賦值給引用必輛。



## Example
```java
public class Person {//父類

	String name = "Mike";
	int age = 18;
	
	public Person() {//父類構造函數
		
		System.out.println("Person talk");
	}
	
	public void show() {
		
		System.out.println(name + "run");
	}
}


public class Worker extends Person {//子類繼承父類

	String name = "Jack";
	int age = 20;
	
	public Worker() {//子類構造函數
		
		System.out.println("Worker talk");
	}
	
	public void work() {
		
		System.out.println(this.name + "," + this.age);//使用本類(子類)成員變量
		System.out.println(super.name + "," + super.age);//使用父類成員變量
	}
}

public class Student extends Person {//子類繼承父類
	
//	String name;
//	int age; -> 以繼承父類成員變量
	
//	public super() {} -> 自動繼承父類中空參數的構造函數，為隱藏的語句。
	
	public Student() {//子類構造函數
		
		//super();
		//通過 super 初始化父類內容時，子類的成員變量並未顯示初始化，等 super() 父類初始化完畢後，才進行子類的成員變量初始化。
		
		System.out.println("Student talk");
		
		//return();
	}
	
	public void study() {
		
		System.out.println(super.name + "," + super.age);//使用父類成員變量
	}

	public void show() {//重寫父類成員方法
		
		System.out.println(super.name + " student run");
	}
}

public class InheritDemo {

	public static void main(String[] args) {
		
		Student s = new Student();
		Worker w = new Worker();
		
		s.study();
		
		s.show();//讀取重寫過後的成員函數
		
		w.work();
		
	}
}

```
### 打印結果
``` 
Person talk
Student talk

Person talk
Worker talk

Mike,18
Mike student run

Jack,20
Mike,18
```
