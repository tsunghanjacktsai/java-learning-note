# Vector 集合

## 演示
```java
import java.util.Enumeration;
import java.util.Iterator;
import java.util.Vector;

public class VectorDemo {

	public static void main(String[] args) {
		
		Vector v = new Vector();
		
		v.addElement("abc1");
		v.addElement("abc2");
		v.addElement("abc3");
		v.addElement("abc4");
		
		//此接口的功能與 Iterator 接口的功能是重複的
		Enumeration en = v.elements();
		
		while(en.hasMoreElements()) {
			
			System.out.println("next element: " + en.nextElement());
		}
		
		Iterator it = v.iterator();
		
		while(it.hasNext()) {
			
			System.out.println("next: " + it.next());
		}
	}
}
```
打印結果
```
next element: abc1
next element: abc2
next element: abc3
next element: abc4
next: abc1
next: abc2
next: abc3
next: abc4
```
