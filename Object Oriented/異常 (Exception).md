# 異常 (Exception)

## 定義
在運行時其發生的不正常情況。
在 Java 中用類的形式對不正常情況進行了描述和封裝對象。
描述不正常情況的類，就稱為異常類。
Before: 正常流程代碼和問題處理代碼結合。
Now: 現在將正常流程代碼和問題處莉代碼分離，提高閱讀性。
簡單來講，就是 Java 通過面向對象的思想將問題封裝成了對象，用異常類對其進行描述。
不同的問題用不同的類進行具體的描述。Example: 角標越界、空指針等。

## 異常體系
- 問題很多，意味著描述的類也很多。
將其共性進行向上抽取，形成異常體系。
異常情況分成兩大類:
1. 一般不可處理的 -> Error
   特點: 由 JVM 拋出的嚴重性問題。
         這種問題發生一般不針對性處理。直接修改程序。
2. 可以處理的 -> Exception

- Throwable(可拋性): 無論是 error，還是異常，問題發生就應該可以拋出，讓調用者知道並處理。
該體系的特點就在於 Throwable 及其所有的子類都具有可拋性。
可拋性通過兩個關鍵字來體現 -> throws & throw
凡是可被這兩個關鍵字所操作的類和對象都具備可拋性。

- **特點:** 子類的後綴名都是用其父類名作為後綴，閱讀性強。
- 對於角標是整數不存在，可以用角標越界表示。
  對於負數為角標的情況，準備用負數角標異常來表示。
  然而，負數角標異常在 Java 中並沒有被定義過。
  可以按照 Java 異常的創建思想，面相對象，將負數角標進行自定義描述，並封裝成對象。
  這種自定義的問題描述稱為自定義異常。
  **注意:** 如果讓一個類稱為異常類，必須要繼承異常體系，因為只有稱為異常體系的子類才具有資格具備可拋性，才可以被兩個關鍵字(throw, throws)所操作。   

## 異常分類
1. 編譯時被檢測異常: 只要是 Exception 和其子類都是，除了特殊子類 RuntimeException
   這種問題一旦出現，希望在編譯時就進行檢測，讓這種問題有對應的處理方式。
2. 編譯時不檢測異常(運行時異常): Exception 中的 RuntimeException 和其子類。
   這種問題的發生，無法讓功能繼續，運算無法進行，更多是因為調用者的原因導致的或者引發了內部狀態的改變導致的。
   這種問題一般不處理，直接編譯通過，在運行時，讓調用者調用時的程序強制停止，讓調用者對代碼進行修正。
- 所以自定義異常時，要麼繼承 Exception，要麼繼承 RuntimeException.

## throws & throw 區別

- throws 使用在函數上。
  throw 使用在函數內。
- throws 拋出的是異常類，可以拋出多個，用逗號隔開。
  throw 拋出的是異常對象。

## 異常捕捉 (catch)
- 定義: 可以對異常進行針對性處理方式。
- 格式: 
```
try {
    //需要被檢測異常的代碼。
} catch (異常類 變量) { //該變量用於接收發生的異常對象
    //處理異常的代碼。
} finally {
    //一定會被執行的代碼
}
```

## 異常處理原則
1. 函數內容如果拋出需要檢測的異常，那麼函數上必須要聲明。
   否則必須在函數內用 try, catch 捕捉，否則編譯失敗。
2. 如果調用到了聲明異常的函數，要麼 try, catch，要麼 throws，否則編譯失敗。
3. 何時使用 catch, 何時使用 throws?
   功能內容可以解決，使用 catch。
   功能內容不能解決，使用 throws 告訴調用者，由調用者解決。
4. 一個功能如果拋出了多個異常，那麼調用時，必須對應多個 catch 進行針對性的處理。
   內部有幾個需要檢測的異常，就拋幾個異常，拋出幾個，就 catch 幾個。

## finally 代碼塊
- 通常用於關閉(釋放)數據庫資源。
- 一定會被執行。

## try, catch, finally 代碼塊組合特點
- try catch finally
- try catch: (多個)當沒有必要資源需要釋放時，可以不用定義 finally。
- try finally: 異常無法直接被處理，但是資源需要被關閉。
```
public void show() throws Exception { //沒有 catch ，必須要聲明

    try { //開啟資源
        //出現異常
        throw new Exception();
    } finally {
        //關閉資源
    }
}
```

## 注意事項
- 子類在重寫父類方法時，父類的方法如果拋出了異常，
  那麼子類的方法只能拋出父類的異常或者該異常的子類。
- 如果父類拋出多個異常，那麼子類只能拋出父類異常的子集。(不能拋額外字定義的異常)
  簡單來講，子類重寫父類只能拋出父類的異常或者子類或者子集。
  **注意:** 如果父類的ㄤ法沒有拋出異常，那麼子類重寫時絕對不能拋，就只能 try。
  
## Example
```
public class ExceptionDemo1 {
	
	public int method(int[] arr, int index) throws NegativeNumException, NullPointerException { //聲明自定義負數異常，可拋出多個
		
		if(arr == null) {
			
			throw new NullPointerException("No Array: " + index);
		}
		
		if(index >= arr.length) {
			
			throw new ArrayIndexOutOfBoundsException("Index is too large: " + index);
		}
		
		//拋出自定義負數異常
		if(index < 0) {
			
			throw new NegativeNumException("Index is too small: " + index);
		}
		
		return arr[index];
	}
}

//自定義負數異常
public class NegativeNumException extends Exception {
	
	NegativeNumException(String message) {
		
		super(message);
	}
}

public class ExceptionDemo {

	public static void main(String[] args) {
		
		int[] arr = {1, 3, 5, 7, 9, 11, 13, 15};
		
		try {
			ExceptionDemo1 e = new ExceptionDemo1();

			int x = e.method(null, -1);
			
			System.out.println("num = " + x);
		
		} catch (NegativeNumException e) { //捕捉異常
			
			System.out.println(e.toString()); //打印自定義異常(包括異常類型)
			System.out.println(e.getMessage()); //打印異常中自定義語句
			e.printStackTrace(); //標示出異常的信息以及異常的位置 -> JVM 默認的異常處理機制
		
		} catch (NullPointerException e) { //可執行多異常捕捉
			
			System.out.println(e.toString());
			e.printStackTrace();
		
		} catch (ArrayIndexOutOfBoundsException e) {
			
			System.out.println(e.toString());
			e.printStackTrace();
			
//			return;
//			System.exit(0); -> 退出 JVM

		} catch (Exception e) { //多 catch 父類的 catch 需放在最下面，否則編譯失敗
			
			System.out.println(e.toString());
			e.printStackTrace();
		
		} finally { //通常用於關閉(釋放)數據庫資源
			
			System.out.println("Finally");
		}
	}
}
```
### 打印結果
```
java.lang.NullPointerException: No Array: -1
java.lang.NullPointerException: No Array: -1
	at OOP.ExceptionDemo1.method(ExceptionDemo1.java:9)
	at OOP.ExceptionDemo.main(ExceptionDemo.java:12)

```
