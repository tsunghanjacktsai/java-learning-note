# JS 函數

## 使用
- 函數: 一個功能的封裝體。
- 定義功能通常需要兩個明確
1. 功能的結果
2. 功能實現中的參與運算的未知內容
- 格式:
```
function 函數名(參數列表) {
    函數體;
    return 返回值;//如果沒有具體返回值，return 語句可以省略不寫
}
```

**注意:** Javascript 沒有重載形式，建議函數爭定義幾個參數就傳幾個實參。

- 函數細節
1. 只要使用函數的名稱就是對這個函數的調用。
2. 函數中有一個數組在對傳入的參數進行存儲。這個數組就是 **argument**。

## 動態函數
- 使用的是 JS 中內置的一個對象 **Function**。
- 使用機率不高。
- 參數列表，函數體都是通過字符串指定的。

## 匿名函數
- 沒有名字的函數
- 通常是函數的簡寫形式
```
<script type="text/javascript">
		function demo() {
			alert("demo run");

			return;
		}

		function add(a, b) {
			
			return a + b;
		}
		
		// demo();
		var sum = add(1, 1);
		// alert("sum = " + sum);
	</script>

	<script type="text/javascript">
		/*
		 * 函數細節
		 */
		 function show(x, y) {

		 	// alert(x + ":" + y);
		 	// alert(arguments.length);
		 	for(var a = 0; a < arguments.length; a++) {
		 		document.write(arguments[a]);
		 	}
		 }

		 show(4, 5, 6, 7, 8);//undefined:undefined
	</script>

	<script type="text/javascript">
		//函數細節2
		function getSum() {

			return 100;
		}

		var sum = getSum();//getSum 函數運行，並將函數返回結果赋值給 sum
		// var sum = getSum;//getSum 本身就是一個函數名，而函數本身在 JS 中就是一個對象。
							//getSum 就是這個函數的引用，將 getSum 這個引用的地址赋給 sum。
							//這時 sum 也指向了這個函數對象。
							//相當於這個函數對象有兩個函數名稱。
							//打印的時候，如果 sum 指向的是函數對象，
							//那麼會將該函數對象以字符串表現形式打印出來
							
		// alert("sum = " + sum);
							  
	</script>
	
	//動態函數
	<script type="text/javascript">
		var add = new Function("x, y", "var sum; sum = x + y; return sum;"); 

		var hello = add(4, 8);
		alert("hello=" + hello);
	</script>
	
	//匿名函數
	<script type="text/javascript">
		var add = function(a, b) {

			return a + b;
		}

		// alert(add(3, 4));

		function jack() {
			
			alert("Hello Jack");
		}

		var vivi = jack;

		//上述代碼可以簡寫成下面格式
		var vivi = function() {
			
			alert("Hello Jack");
		}
	</script>
```

