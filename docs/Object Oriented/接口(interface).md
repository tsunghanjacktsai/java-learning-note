# 接口 (interface)

## 使用
- 當一個抽象類中的方法都是抽象的時候，這時可以將該抽象類用另一   種形式定義和表示，就是接口 interface。
- 格式: interface {}
- 接口中的成員修飾符是固定的
1. 成員常量: public static final
2. 抽象方法: public abstract
3. 接口中的成員都是 public 的

## 特性
- 接口是對外暴露的規則。(Example: 電腦的 USB 接口)
- 接口是程序的功能擴展。(Example: USB、滑鼠擴展電腦功能)
- 接口的出現降低耦合性(聯繫程度)。(Example: USB接口的出現可以降低電腦與滑鼠硬盤等的聯繫)
- 接口可以用來多實現。
- 接口與接口之間是繼承關係(不是實現)，而且接口可以實現**多繼承**。
- 一個類在繼承另一個類的同時，還可以實現多個接口。

## 實現 (implements)
- 類與類之間是繼承關係，類與接口直接是實現關係。
- 接口不可以實例化，
  只能由實現接口的子類且重寫了接口中所有的抽象方法後，該子類才可以實例化，
  否則，這個子類就是一個抽象類。

## 接口和抽象類的異同點
- 相同點:
1. 都是不斷向上抽取而來。
-  相異點:
1. 抽象類需要被繼承，而且只能單繼承
   接口需要被實現，接口需要被實現，而且可以多實現。
2. 抽象類中可以定義抽象方法和非抽象方法，子類繼承後，可以直接使用非抽象方法。
   接口中只能定義抽象方法，必須由子類去實現。
3. 抽象類的繼承，是 is a 關係，在定義該體系的**基本**共性內容。
   接口的實現是 like a 關係， 在定義體系的**額外**功能。

## Example
```java
//接口格式
interface Interface1 {
    
        public static final int NUM = 5;
        
        public abstract void show1();
        public abstract void show2();
}

interface Interface2 {

        public abstract int add(int a, int b);
}

//多實現
public class InterfaceImpl implements Interface1, Interface2 {
    
    public void show1() {
    
    }

    public void show2() {
    
    }
    
    public int add(int a, int b) {
    
    return a + b;
    }
}

//繼承(單繼承) + 實現
public class A {

    public void method() {
    
    }
}

public class B extends A implements Interface1, Interface2 {

}

//接口之間的多繼承關係
interface AA {

    public abstract void show();
}

interface BB {

    public abstract void methods();
}

//接口多繼承
interface CC extends AA, BB {
    
    public abstract void function();
}

//重寫三個方法
public class DD implements CC {

    public void show() {
    
    }
    
    public void method() {
    
    }
    
    public void function() {
    
    }
}
```
---
```
public abstract class InterfaceCannie {//基本功能
	
	public abstract void bark();
}

interface InterfacePlay {//額外功能
	
	public abstract void play();
}

public class InterfaceDog extends InterfaceCannie implements InterfacePlay {

	public void bark() {//繼承且重寫基本功能
		
		System.out.println("Ruff Ruff");
	}
	
	public void play() {//實現額外功能
		
		System.out.println("I wanna play a game");
	}
}

public class InterfaceWolf extends InterfaceCannie {//僅繼承並重寫功能，並不需要額外功能的實現
	
	public void bark() {
		
		System.out.println("HaHa HaHa");
	}
}

public class InterfaceDemo {
    
    public static void main(String[] args){
    
        InterfaceDog d = new InterfaceDog();
        InterfaceWolf w = new InterfaceWolf();
        
        d.bark();
        d.play();
        w.bark();
    }
}
```
### 打印結果
```
Ruff Ruff
I wanna play a game
HaHa HaHa
```
