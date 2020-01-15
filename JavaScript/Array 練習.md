# Array 練習

## 演示
```javascript
//out.js
/**
 * 打印指定參數到頁面上
 */

function print(x) {
	document.write(x);
}

function println(x) {
	document.write(x + "<br>")
}

<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<script type="text/javascript" src="JS/out.js"></script>
	<script type="text/javascript">
		//用數組實現 js 中的堆棧或者隊列數據結構
		var arr = ["Jack"];

		// arr.unshift("abc1", "abc2", "abc3", "abc4", "abc5");
		//abc1,abc2,abc3,abc4,abc5,Jack

		arr.unshift("abc1");
		arr.unshift("abc2");
		arr.unshift("abc3");
		arr.unshift("abc4");
		arr.unshift("abc5");
		//abc5,abc4,abc3,abc2,abc1,Jack

		println(arr);

		//堆棧
		// println(arr.pop());
		// println(arr.pop());
		// println(arr.pop());
		// println(arr.pop());
		// println(arr.pop());
		// println(arr.pop());

		//Jack abc1 abc2 abc3 abc4 abc5

		//隊列
		println(arr.shift());		
		println(arr.shift());		
		println(arr.shift());		
		println(arr.shift());		
		println(arr.shift());
		println(arr.shift());
		//abc5 abc4 abc3 abc2 abc1 Jack	
	</script>
</body>
</html>
```
---
```javascript
//out.js
/**
 * 打印指定參數到頁面上
 */

function print(x) {
	document.write(x);
}

function println(x) {
	document.write(x + "<br>")
}

//max.js
//數組獲取最大值
Array.prototype.getMax = function() {

	var temp = 0;
	for(var x = 1; x < this.length; x++) {
		if(this[x] > this[temp]) {
			temp = x;
		}
	}

	return this[temp];
}

//數組的字符串表現形式
//定義 toString 方法
Array.prototype.toString = function() {

	return "[" + this.join(", ") + "]";
}

<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<script type="text/javascript" src="JS/out.js"></script>
	<script type="text/javascript" src="JS/max.js"></script>
	<script type="text/javascript">
		//自訂義新功能，用原型屬性
		var arr = ["Jack", "Vivian", "Jervis", "Jacky", "Mike"];

		var maxVal = arr.getMax();
		println("Max Value: " + maxVal);//Max Value: Vivian
		println(arr.toString());//[Jack, Vivian, Jervis, Jacky, Mike]
	</script>
	
</body>
</html>
```
