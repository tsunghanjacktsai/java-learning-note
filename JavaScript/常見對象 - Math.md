# 常見對象 - Math

## 使用
- 該對象中的方法都是**靜態**的，不需要 new，直接 Math   調用即可。
- ceil 方法: 返回大於等於指定參數的最小整數。
- floor 方法: 返回小於等於指定數據的最大整數。
- round 方法: 四捨五入。

## 演示
```javascript
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<script type="text/javascript" src="JS/out.js"></script>
	<script type="text/javascript">
		var num1 = Math.ceil(15.34);
		var num2 = Math.floor(15.34);
		var num3 = Math.round(15.34);

		println("num1 = " + num1);//num1 = 16
		println("num2 = " + num2);//num2 = 15
		println("num3 = " + num3);//num3 = 15

		var num4 = Math.pow(10, 2);
		println("num4 = " + num4);//num4 = 100

		println("<hr>");

		for(var x = 0; x < 3; x++) {
			// var num = Math.floor(Math.random() * 10 + 1);
			var num = parseInt(Math.random() * 10 + 1);
			println(num);
		}
		//2 3 6
	</script>
</body>
</html>
```
