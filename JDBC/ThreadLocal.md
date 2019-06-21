# ThreadLocal

## ThreadLocal API
ThreadLocal 類只有三個方法
- void set(T value): 保存值
- T get(): 獲取值
- void remove(): 移除值

## Example
```java
package demo9;

import java.util.HashMap;
import java.util.Map;

import org.junit.Test;

/**
 * ThreadLocal 對象的用法
 * @author jack870131
 *
 */
public class Demo9 {
	@Test
	public void fun1() {
		final ThreadLocal<String> tl = new ThreadLocal<>();
//		tl.set("hello"); //存
		
		new Thread() {
			public void run() {
				tl.set("內部類存");
//				System.out.println("內容類:" + tl.get());
			}
		}.start();
		try {
			Thread.sleep(1000);
		} catch(Exception e) {
			e.printStackTrace();
		}
		System.out.println(tl.get()); //取
	}
	
	@Test
	public void fun2() {
		Map<Thread, String> map = new HashMap<>();
		map.put(Thread.currentThread(), "hello");
		System.out.println(map.get(Thread.currentThread()));
		
		new Thread() {
			public void run() {
				System.out.println(Thread.currentThread().getName());
				System.out.println(map.get(Thread.currentThread()));
			}
		};
	}
	
	/**
	 * ThreadLocal 內部是一個 Map
	 * @author jack870131
	 *
	 * @param <T>
	 */
	class TL<T> {
		private Map<Thread, T> map = new HashMap<Thread, T>();
		
		public void set(T data) {
			//使用當前線程做 key
			map.put(Thread.currentThread(), data);
		}
		
		public T get() {
			return map.get(Thread.currentThread());
		}
		
		public void remove() {
			map.remove(Thread.currentThread());
		}
	}
}

/**
 * ThreadLocal通常用在一个类的成员上
 * 多个线程访问它时，每个线程都有自己的副本，互不干扰
 * Spring中把Connection放到了ThreadLocal中
 * @author jack870131
 */
class User {
	 private ThreadLocal<String> usernameTl = new ThreadLocal<>();
}
```
