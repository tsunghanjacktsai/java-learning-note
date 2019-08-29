# Map 集合練習

## 1.
**"fdgavcbaacdfa"**，獲取該字符串中，每一個字母出現的次數。要求打印結果是: a(2)b(1)...
- 對於結果的分析發現，字母和次數之間存在著映射的關係，而且這種關係很多，
需要存儲，能存儲映射關係的容器有數組和 Map 集合。------>**使用 Map 集合**。
- 發現可以保證唯一性的一方具備著順序如 abc..... -------> **使用 TreeMap 集合**。
- 這個集合中最終應該存儲的是字母和次數的對應關係。
1. 因為操作的是字符串中的字母，所以先將字符串變成字符數組。
2. 遍歷字符數組，用每一個字母作為鍵去查 Map 集合這個表。
   如果該字母鍵不存在，就將該字母作為鍵 1 作為值存儲到 Map 中。
   如果該字母鍵存在，就將該字母鍵對應值取出並加 1 ，再將該字母和 +1 後的值存儲到 Map 集合中，
   鍵值相同會覆蓋，這樣就記錄了該字母的次數。
3. 遍歷結束，Map 集合就紀錄所有字母的出現次數。

```
import java.util.Iterator;
import java.util.Map;
import java.util.TreeMap;

public class MapTest {

	public static void main(String[] args) {
		
		String str = "fd-gav-cBa-aCDfa";
		
		String s = getCharCount(str);
		
		System.out.println(s);
	}

	public static String getCharCount(String str) {

		//將字符串變成字符數組
		char[] ch = str.toCharArray();
		
		//定義 Map 集合表
		Map<Character, Integer> map = new TreeMap<>();
		
		for(int i = 0; i < ch.length; i++) {
			
			if(!(ch[i] >= 'a' && ch[i] <= 'z' || ch[i] >= 'A' && ch[i] <= 'Z'))
				continue;
				
			//將數組中的字母作為鍵去查 Map 表
			Integer value = map.get(ch[i]);
			
			int count = 1;
			
			//判斷值是否為空
			if(value != null) {
				
				count = value + 1;
			}
			
			map.put(ch[i], count);
			
			/*
			if(value == null) {
				
				map.put(ch[i], 1);
			} else {
				
				map.put(ch[i], value + 1);
			}
			*/
			
		}
		
		return mapToString(map);
	}
	
	private static String mapToString(Map<Character, Integer> map) {
		
		StringBuilder sb = new StringBuilder();//容器
		
		Iterator<Character> it = map.keySet().iterator();
		
		while(it.hasNext()) {
			
			Character key = it.next();
			Integer value = map.get(key);
			
			sb.append(key + "(" + value + ")");
		}
		
		return sb.toString();
	}
}
```
打印結果
```
B(1)C(1)D(1)a(4)c(1)d(1)f(2)g(1)v(1)
```

---

## 2.
Map 在有映射關係時，可以優先考慮。
在**查表法**中的應用較為多見。
