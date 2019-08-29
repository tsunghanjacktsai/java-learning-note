# 獲取 Class 中的字段

## 演示
```
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

import java.lang.reflect.Field;

public class ReflectDemo3 {

	public static void main(String[] args) throws Exception {

		getFieldDemo();
	}
	
	public static void getFieldDemo() throws Exception {
		
		Class c = Class.forName("Reflect.ReflectPerson");
		
		Field field = null;  //c.getField("age"); //只能獲取公有的
		
		field = c.getDeclaredField("age");//只獲取本類，但包含私有
		
		//對私有字段的訪問取消權限檢查，暴力訪問 -> 不推薦
		field.setAccessible(true);
		
		Object obj = c.newInstance();
		
		field.set(obj, 90);//將 age 設置為 89
		
		Object o = field.get(obj);
		
		System.out.println(o);
	}

}
```
打印結果
```
Person run
90
```
