# prototype 練習

## 練習
1. 練習1: 給字符串添加一個功能，將字符串變成一個字符數組。
2. 練習2: 給字符串添加一個功能，將字符串進行反轉。

## 演示
```
//array.js
//將字符串返回一個數組
String.prototype.toCharArray = function() {

	//定義一個數組
	var arr = [];
	//將字符串中每一位字符存儲到字符數組中
	for(var x=0; x < this.length; x++) {

		arr[x] = this.charAt(x);
	}

	return arr;
}

//將字符串進行反轉
String.prototype.reverse = function() {

	//將位置置換功能進行封裝，並定義到反轉功能內部
	function swap(arr, a, b) {
		var temp = arr[a];
		arr[a] = arr[b];
		arr[b] = temp;
	}
	var chs = this.toCharArray();

	for(var x = 0, y = chs.length - 1; x < y; x++, y--) {

		swap(chs, x, y);
	}

	return chs.join("");
}

<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<script type="text/javascript" src="JS/out.js"></script>
	<script type="text/javascript" src="JS/array.js"></script>
	<script type="text/javascript">
		
		var str = "abcde";

		println(str.toCharArray());
		println(str.reverse());
	</script>
</body>
</html>
```
