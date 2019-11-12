# Math 類

## 使用
- 提供了操作數學運算的方法，都是靜態的。
- 常用方法:
ceil(): 返回大於參數的最小整數。
floor(): 返回小於參數的最大整數。
round(): 返回四捨五入的整數。
pow(a, b): a 的 b 次方。
random(): 該值大於等於 0.0 且小於 1.0，返回值是一個偽隨機 double 值。(有其一定規律)

## 演示
```java
public class MathDemo {

	public static void main(String[] args) {

		double d1 = Math.ceil(12.56);
		double d2 = Math.floor(12.56);
		double d3 = Math.round(12.56);
		double d4 = Math.pow(10, 2);
		
		print("d1 = " + d1);
		print("d2 = " + d2);
		print("d3 = " + d3);
		print("d4 = " + d4);
		print("");
		
		for(int i = 0; i < 5; i++) {
			
			double d5 = (int)(10 * Math.random() + 1);//1 到 10 的隨機整數
			print("d5 = " + d5);
		}
	}

	public static void print(String string) {
		
		System.out.println(string);
	}
}
```
打印結果
```
d1 = 13.0
d2 = 12.0
d3 = 13.0
d4 = 100.0

d5 = 4.0
d5 = 1.0
d5 = 4.0
d5 = 8.0
d5 = 7.0
```
