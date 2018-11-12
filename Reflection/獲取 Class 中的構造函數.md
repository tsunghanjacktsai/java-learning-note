# 獲取 Class 中的構造函數

## 使用
- 早期: new 對象的時候，先根據被 new 的類名尋找該類的**字節碼文件**，並加載進**內存**，創建該字節碼文件**對象**，並接著創建該字節碼文件所**對應的對象**。
- 現在: 找尋該名稱類文件，並加載進內存，產生 **Class 對象**，接著產生該類的對象。
- 當獲取指定名稱對應類中所體現的對象時，
該對象初始化不使用空參數構造的方法
-> 既然是通過指定的構造函數進行對象的初始化，
所以應該**先獲取該構造函數**，通過**字節碼文件對象**即可完成。
-> getConstructor(parameterTypes);

## 演示
```
package Reflect;

public class ReflectPerson {
	
	private String name;
	private int age;
	
	public ReflectPerson(String name, int age) {
		
		this.name = name;
		this.age = age;
		System.out.println("Person param run @ " + this.name + this.age);
	}
	
	public ReflectPerson() {
		
		System.out.println("Person run");
	}
	
	public void show() {
		
		System.out.println(name + " @ " + age);
	}
	
	private void method() {
		
		System.out.println("method run");
	}
	
	public void paramMethod(String str, int num) {
		
		System.out.println("paramMethod run" + str + " @ " + num);
	}
	
	public static void staticMethod() {
		
		System.out.println("static method run");
	}
}

package Reflect;

import java.lang.reflect.Constructor;

public class ReflectDemo2 {

	public static void main(String[] args) throws Exception {

		createNewObject();
		createNewObject_2();
	}
	
	public static void createNewObject_2() throws Exception {

//		Reflect.ReflectPerson p = new Reflect.ReflectPerson("Jack", 20);
		
		/*
		 * 當獲取指定名稱對應類中所體現的對象時，
		 * 該對象初始化不使用空參數構造的方法
		 * -> 既然是通過指定的構造函數進行對象的初始化，
		 * 所以應該先獲取該構造函數，通過字節碼文件對象即可完成。
		 * -> getConstructor(parameterTypes);
		 */
		String name = "Reflect.ReflectPerson";
		//找尋該名稱類文件，並加載進內存，並產生 Class 對象
		Class c = Class.forName(name);
		
		Constructor constructor = c.getConstructor(String.class, int.class);
		
		//通過該構造器對象的 newInstance 方法進行對象的初始化
		Object obj = constructor.newInstance("Jack", 20);
	}

	public static void createNewObject() throws ClassNotFoundException, InstantiationException, IllegalAccessException {
		
		//早期 
//		Reflect.ReflectPerson p = new Reflect.ReflectPerson();
		
		//現在
		String name = "Reflect.ReflectPerson";
		//找尋該名稱類文件，並加載進內存，並產生 Class 對象
		Class c = Class.forName(name);
		//產生該類的對象
		Object obj = c.newInstance();
		
	}
}
```
## 打印結果
```
Person run
Person param run @ Jack @ 20
```
