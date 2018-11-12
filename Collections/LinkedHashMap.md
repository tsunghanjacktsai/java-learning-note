# LinkedHashMap

## 定義
**哈希鏈表**: 依照存儲順序。

## 演示
```
import java.util.HashMap;
import java.util.Iterator;
import java.util.LinkedHashMap;
import java.util.Map;

public class LinkedHashMapDemo {

	public static void main(String[] args) {
		
		HashMap<Integer, String> hm = new LinkedHashMap<>();//哈希鏈表(依照存儲順序)
		
		hm.put(20, "Jack");
		hm.put(40, "Vivian");
		hm.put(60, "Mike");
		hm.put(80, "Jervis");
		
		Iterator<Map.Entry<Integer, String>> it = hm.entrySet().iterator();
		
		while(it.hasNext()) {
			
			Map.Entry<Integer, String> me = it.next();
			
			Integer key = me.getKey();
			String value = me.getValue();
			
			System.out.println(key + " @ " + value);
		}
	}

}
```
打印結果
```
20 @ Jack
40 @ Vivian
60 @ Mike
80 @ Jervis
```
