# 獲取 Class 中的方法

## 演示
```java
package Reflect;

public class ReflectPerson {
	
	private String name;
	private int age;
	
	public ReflectPerson(String name, int age) {
		
		this.name = name;
		this.age = age;
		System.out.println("Person param run @ " + this.name + " @ " + this.age);
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
import java.lang.reflect.Method;

public class ReflectDemo4 {

	public static void main(String[] args) throws Exception {

		getMethodDemo();
		getMethodDemo_2();
		getMethodDemo_3();
	}

	public static void getMethodDemo_3() throws Exception {

		Class c = Class.forName("Reflect.ReflectPerosn");
		
		Method method = c.getMethod("paramMethod", String.class, int.class);
		
		Object obj = c.newInstance();
		
		method.invoke(obj, "Jack", 20);
	}

	public static void getMethodDemo_2() throws Exception {

		Class c = Class.forName("Reflect.ReflectPerson");
		
		Method method = c.getMethod("show", null);//獲取空參數一般方法
		
//		Object obj = c.newInstance();
		Constructor constructor = c.getConstructor(String.class, int.class);
		Object obj = constructor.newInstance("Jack", 20);
		
		method.invoke(obj, null);
	}

	//獲取指定 class 中的所有公共函數
	public static void getMethodDemo() throws Exception {

		Class c = Class.forName("Reflect.ReflectPerson");
		
		Method[] methods = null; //c.getMethods();//獲取的都是公有的方法
		methods = c.getDeclaredMethods();//只獲取本類中所有方法，包含私有
		
		for(Method method: methods) {
			
			System.out.println(method);
		}
	}

}
```
打印結果
```
//getMethodDemo()
public static void Reflect.ReflectPerson.staticMethod()
private void Reflect.ReflectPerson.method()
public void Reflect.ReflectPerson.show()
public void Reflect.ReflectPerson.paramMethod(java.lang.String,int)

//getMethodDemo_2()
Person param run @ Jack @ 20
Jack @ 20

//getMethodDemo_3()
Person run
paramMethod runJack @ 20
```
