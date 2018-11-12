# 全局方法 & Number 對象

## 演示
```
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
		// var val = parseInt("12abc");//value = 12

		// println("value = " + val);

		var num = parseInt("110", 2);//num = 6 -> 二進制轉十進制
		println("num = " + num);

		var num1 = parseInt("0x3c", 16);//num1 = 60
		println("num1 = " + num1);

		//將十進制轉成其他進制，使用數字對象完成
		var num3 = new Number(6);
		println("num3 = " + num3.toString(2));//num3 = 110

		var num4 = 60;
		println("num4 = " + num4.toString(16));//num4 = 3c
	</script>
</body>
</html>
```

## for in 語句
- 格式:
for(變量 in 對象)//對對象進行變量的語句
{}
```
//for in 語句
		var arr = [32, 80, 65];

		for(i in arr) {
			println("i = " + i);
		}
		//i = 0 i = 1 i = 2
```
