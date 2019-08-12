# 多態 (Polymorphism)
 
## 定義
- 某一類事物的多種存在型態。
- 多態在代碼中的體現: 父類或者接口的引用指向其子類的對象。

## 優點
- 提高了代碼的擴展性 -> 前期定義的代碼可以使用後期的內容。

## 缺點
- 前期定義的內容不能使用(調用)後期子類的特有內容。

## 使用前提
1. 必須有關係，繼承，實現。
2. 要有重寫。

## 轉型
- 向上轉型: 限制對特有功能的訪問，將子類型隱藏，就不用使用子類的特有方法。
- 向下轉型: 為了使用子類中的特有方法。
- **注意:** 對於轉型，自始自終都是子類對象在做著類型的變化。

## instanceof
- 用於判斷對象的具體類型，只能用於引用數據類型。
- 通常在向下轉型前用於健壯性的判斷。

## 多態成員的特點
- 成員變量
編譯時: 參考引用型變量所屬的類中是否有調用的成員變量，有則編譯通過，沒有則編譯失敗。
運行時: 參考引用型變量所屬的類中是否有調用的成員變量，並運行該所屬類中的成員變量。
簡單來講，編譯和運行都參考**等號左邊**。(多於考試出現)
- **成員函數(非靜態)**
編譯時: 參考引用型變量中是否有調用的函數，有則編譯通過，沒有則編譯失敗。
運行時: 參考的是對向所屬的類中是否有調用的函數。
簡單來講，編譯看**等號左邊**，**運行看等號右邊**。
- 靜態函數
編譯時: 參考引用型變量所屬類中是否有調用的靜態方法。
運行時: 參考引用型變量所屬類中是否有調用的靜態方法。
簡單來講，編譯和運行都看左邊。
P.S. 其實對於靜態方法，是不需要對象的。直接用類名調用即可。

## Example
```java
public abstract class Animal {

    public abstract void eat();
}

public class Dog extends Animal {

    public void eat() {
    
        System.out.println("I like to eat meat");
    }
    
    public void bark() {//特有內容
    
        System.out.println("Ruff Ruff");
    }
}

public class Cat extends Animal {
    
    public void eat() {
    
        System.out.println("I like to eat fish");
    }
    
    public void play() {
    
        System.out.println("I like to play rats");
    }
}

public class PolymorphismDemo {

	public static void main(String[] args) {
		
		Animal a = new Cat();
		
		Animal b = new Dog();
	
		method(a);
		Cat c = (Cat) a;//想調用特有功能就要進行向下轉型
		c.play();
		
		method(b);
		Dog d = (Dog) b;
		d.bark();
	}
	
		public static void method(Animal a) {//Animal a = new Dog() or new Cat();
		
		a.eat();
		
		if(a instanceof Cat) {//instanceof: 用於判斷對象的具體類型，只能用於引用數據類型。
			Cat c = (Cat) a;//想調用特有功能就要進行向下轉型
			c.play();
		} else if (a instanceof Dog) {
			Dog d = (Dog) a;
			d.bark();
		}
	}
}
}
```
### 打印結果
```
I like to eat fish
I like to play rats
I like to eat meat
Ruff Ruff
```
