# 抽象類 (abstract)

## 使用時機
當一個類在描述事物時，沒有足夠的信息來描述這個事物，就定義為抽象的。

## 特點
1. 方法只有聲明沒有實現時，該方法就是抽象方法，需要被 abstract 修飾。
  抽象方法必須定義在抽象類中。該類必須也被 abtract 修飾。
2. 抽象類不可以被實例化。因為調用抽象方法沒意義。
3. 抽象類必須有其子類重寫了所有的抽象方法後，該子類才可以實例化。否則，該子類還是抽象類。
4. 抽象類中有構造函數，用於給子類進行初始化。
5. 抽象類中可以不定義抽象方法，但是很少見，目的就是不讓該類創建對象。( AWT 的適配器就是這種類)
  通常這個類中的方法有方法體，但是卻沒有內容。
6. 抽象關鍵字不可以和一些關鍵字共存 => private, static, final
7. 抽象類和一般類的異同點
   **相同點** 
   抽象類和一般類都是用來描述事務的，都在內部定義了成員。
   **不同點** 
   一般類有足夠的信息描述事務，抽象類描述事物的信息有可能不足。
   一般類中不定義抽象方法，只能定義非抽象方法。抽象類中可以定義抽象方法，同時也可以定義非抽象方法。
   一般類可以被實例化。抽象類不可以被實例化。
8. 抽象類一定是父類，因為需要子類重寫其方法後才可以對子類實例化。

## Example
```java
public abstract class AbstractCannie {//抽象類同時也是父類 -> 犬科
	
	public abstract void bark();//犬科共有方法 -> 吠叫
}

public class AbstractWolf extends AbstractCannie {
	
	public void bark() {//定義犬科抽象方法
		
		System.out.println("haha");
	}

}

public class AbstractDog extends AbstractCannie {

	public void bark() {//定義犬科抽象方法
		
		System.out.println("ruff ruff");
	}
}
```
---
```java
public abstract class AbstractEmployee {//抽象類、父類
	
	private String name;
	private int id;
	private int salary;
	
	public AbstractEmployee(String name, int id, int salary) {
		
		this.name = name;
		this.id = id;
		this.salary = salary;
	}
	
	public abstract void work();
	
	public String getName() {
		
		return name;
	}
	
	public void setName(String name) {
		
		this.name = name;
	} 
	
	public int getId() {
		
		return id;
	}
	
	public void setId(int id) {
		
		this.id = id;
	}
	
}

public class AbstractManager extends AbstractEmployee {
	
	private int bonus;
	
	public AbstractManager(String name, int id, int salary, int bonus) {
		
		super(name, id, salary);//調用父類的構造函數
		this.bonus = bonus;//新增自己所需的變量
	}
	
	public void work() {
		
		System.out.println("Manager");
	}

}

public class AbstractEngineer extends AbstractEmployee {
	
	public AbstractEngineer(String name, int id, int salary) {
		
		super(name, id, salary);//調用父類的構造函數
	}

	public void work() {
		
		System.out.println("Engineer");
	}
}

public class AbstractDemo {

	public static void main(String[] args) {
		
		AbstractEngineer e = new AbstractEngineer("Jack", 100, 1000);
		AbstractManager m = new AbstractManager("Jervis", 101, 1000, 10000);
		
		e.setName("Mike");//對象自動繼承父類所用有的成員方法
		m.setId(102);
		
		System.out.println(e.getName());
		System.out.println(m.getId());
	}
}
```
### 打印結果
```
Mike
102
```
