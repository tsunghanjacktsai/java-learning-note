# ArrayStream

## 使用
操作數組。

## 操作數組對象
- **操作字節數組:** ByteArrayInputStream & ByteArrayOutputStream
- **操作字符數組:** CharArrayReader & CharArrayWrite
- **操作字符串:** StringReader & StringWriter

## 演示
```
import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;

public class ByteArrayStreamDemo {

	public static void main(String[] args) {

		ByteArrayInputStream bis = new ByteArrayInputStream("abcdefg".getBytes());
		
		ByteArrayOutputStream bos = new ByteArrayOutputStream();
		
		//對象本身即是數組，可不必創建數組
		int ch = 0;
		
		while((ch = bis.read()) != -1) {
			
			bos.write(ch);
		}
		
//		bis.close();//可以不用關流
		
		System.out.println(bos.toString());
	}

}
```
打印結果
```
abcdefg
```
