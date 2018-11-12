# JS 運算符

## 運算符
1. 算數運算符: + - * / % ++ --
2. 賦值運算符: = += -= *= /= %=
3. 比較運算符: > < >= <= != ==
   運算完的結果只有 true or false
4. 邏輯運算符: & && | || ^ !
   用來連接兩個 boolean 型的表達式。
5. 位運算符: & | ^ >> << >>>
6. 三元運算符: ? :
```
<!--運算符-->
	<script type="text/javascript">
		//1. 算數運算符
		var a = 56800;
		// alert("a = " + a/1000*1000);//56800

		var a1 = 2.5;
		var b1 = 9.5;
		// alert("a1 + b1 = " + (a1 + b1));
		
		// alert("12" - 1);//11
		// alert("12" + 1);//121
		// alert(true + 1);//2
						//因為 JS 中 false 就是0，或者null
						//非0，非null，就是 true，默認用1表示

		// alert(2 % 5);//2

		var n = 3, m;
		// m = n++;//n = 4 m = 3
		m = ++n;//n = 4 m = 4
		// alert("n = " + n + " m = " + m);
		//2. 賦值運算符
		var i = 4;
		// i = i + 2;//6
		i += 2;//6
		// alert("i = " + i);
		
		//3. 比較運算符
		var z = 3;
		// alert("z = " + z);//true
		
		//4. 邏輯運算符
		var t = 5;
		// alert(t > 3 && t < 6);//true
		// alert(t > 3 & t < 6);//1	
		// alert(!true);//false	
        
        //5. 位運算符
		var c = 6;
		
		// alert(c & 3);//2
		// alert(5 ^ 3 ^ 3);//3
		// alert(c >>> 1);//6 / (2^1) = 3
		// alert(c << 2);//6 * (2^2) = 24
		
		//6. 三元運算符
		// 3 > 0 ? alert(yes) : alert(no);
		// alert(3 > 10 ? 100 : 200);//false -> 200
		
		//undefined & typeof
		<script type="text/javascript">
		/*
		 * 1. undefined: 未定義
		 */
		 var y;
		 // alert(y);//undefined

		 //要想獲取具體值的類型，可以通過 typeof 來完成
		 /*
		 alert(typeof("Hello world"));//string
		 alert(typeof("Hello") == 'string');//true -> 判斷類型
		 alert(typeof(2.5));//number
		 alert(typeof(true));//boolean
		 alert(typeof(78));//number
		 alert(typeof('9'));//string
		 */
	    </script>
```
