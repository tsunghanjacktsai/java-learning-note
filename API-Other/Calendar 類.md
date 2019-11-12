# Calendar 類

## 演示
```java
import java.util.Calendar;

public class CalendarDemo {

	public static void main(String[] args) {
		
		show();
	}

	private static void show() {
		
		Calendar c = Calendar.getInstance();
		
		int year = c.get(Calendar.YEAR);
		int month = c.get(Calendar.MONTH) + 1;
		int day = c.get(Calendar.DAY_OF_MONTH);
		int week = c.get(Calendar.DAY_OF_WEEK);//星期六為最後一天
		
		System.out.println(year + "-" + month + "-" + day + "-" + getWeek(week));
	}

	public static String getWeek(int i) {

		String[] weeks = {"","Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"};
		
		return weeks[i];
	}
}
```
打印結果
```
2018-1-29-Mon
```
---
計算某年的二月有幾天
```
import java.util.Calendar;

public class CalendarTest {

	public static void main(String[] args) {
		
		Calendar c = Calendar.getInstance();
		
		int year = 2016;
		showDays(year);
	}
	
	public static void showDays(int year) {
		
		Calendar c = Calendar.getInstance();
		c.set(year, 2, 1);//三月一號
		
		c.add(Calendar.DAY_OF_MONTH, -1);//減一天以求出二月最後一天
		
		showDate(c);
	}
	
	public static void showDate(Calendar c) {
		
		int day = c.get(Calendar.DAY_OF_MONTH);
		
		System.out.println(day);
	}
}
```
打印結果
```
29
```
