# Date 類

## 使用
- 日期對象和毫秒值之間的轉換
- 毫秒值 -----> 日期對象
1. 通過 Date 對象的構造方法 new Date(timeMillist);
2. 還可以通過 setTime 設置。
   因為可以通過 Date 對象的方法對該日期中的各個字段(年月日等)進行排序。
- 日期對象 -----> 毫秒值
1. getTime 方法。
   因為可以通過具體的數值進行運算。
- 對日期對象進行格式化
1. 日期對象 --> 日期格式的字符串
   使用的是 DateFormat 類中的 format 方法。
2. 日期格式的字符串 --> 日期對象
   使用的是 DateFormat 類中的 parse() 方法

## 演示
```
import java.text.DateFormat;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class DateDemo {

	public static void main(String[] args) throws ParseException {

		long time = System.currentTimeMillis();
		System.out.println(time);//1517206070576
		
		Date date = new Date();//當前日期和時間封裝成日期對象
		System.out.println(date);//Mon Jan 29 14:07:50 CST 2018
		
		//對日期對象進行格式化
		method();
		method_2();
	}
	
	//日期對象 --> 日期格式的字符串
	public static void method() {
		
		Date date = new Date();
		
		//獲取日期格式對象，具備默認風格。FULL, LONG 等可以指定風格
		DateFormat dateFormat = DateFormat.getDateInstance(DateFormat.FULL);
		dateFormat = DateFormat.getDateTimeInstance(DateFormat.LONG, DateFormat.LONG);//帶時間的格式
		
		//自訂義風格
		dateFormat = new SimpleDateFormat("yyyy--MM--dd");
		
		String str_date = dateFormat.format(date);
		
		System.out.println(str_date);
	}
	
	//日期格式的字符串 --> 日期對象
	public static void method_2() throws ParseException {

		String str_date = "2018-1-29";
		
		DateFormat dateFormat = DateFormat.getDateInstance(DateFormat.LONG);
		
		dateFormat = new SimpleDateFormat("yyyy-MM-dd");
		
		Date date = dateFormat.parse(str_date);
		
		System.out.println(date);
	}
}
```
打印結果
```
1517209281249
Mon Jan 29 15:01:21 CST 2018
2018--01--29
Mon Jan 29 00:00:00 CST 2018
```
---
"2018-03-17" 到 "2018-04-16" 中間有多少天?

- 思路: 兩個日期相減 -> 必須要有兩個可以進行相減運算的數 -> 毫秒值 -> 通過 Date 對象獲取 -> 將字符串轉成 Date 對象
1. 將日期格式的字符串轉成 Date 對象
2. 將 Date 對象轉成毫秒值
3. 相減，變成天數

**Answer**
```
import java.text.DateFormat;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class DateClass {

	public static void main(String[] args) throws ParseException {

		test("2018-03-17", "2018-04-16");
	}

	public static void test(String str_date1, String str_date2) throws ParseException {

		//將日期轉成字符串對象
		//定義日期格式對象
		DateFormat dateFormat = DateFormat.getInstance();
		dateFormat = new SimpleDateFormat("yyyy-MM-dd");
		
		Date date1 = dateFormat.parse(str_date1);
		Date date2 = dateFormat.parse(str_date2);
		
		long time1 = date1.getTime();
		long time2 = date2.getTime();
		
		long time = Math.abs(time1 - time2);
		
		int day = getDay(time);
		
		System.out.println(day);
	}

	public static int getDay(long time) {

		int day = (int)(time/1000/60/60/24);
		
		return day;
	}
}
```
打印結果
```
30
```
