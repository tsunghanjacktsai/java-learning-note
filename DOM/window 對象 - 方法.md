# window 對象 - 方法

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
		function windowMethodDemo() {

			// var c = confirm("Are u sure?");
			// println(c);//true

			// setTimeout("alert('Hello World')", 3000);//三秒後顯示一彈窗
			timeid = setInterval("alert('Hello World')", 3000);//每隔三秒顯示彈窗，不斷循環
		}

		function stopTime() {

			clearInterval(timeid);
		}

		function windowMove() {

			// moveBy(10, 10);
			// moveTo(50, 50);
		}

		function windowOpen() {

			window.open("https://www.baidu.com");
		}
	</script>
	<!--<input type="button" value="演示window對象中的方法" onclick="windowMethodDemo()">-->
	<!--<input type="button" value="演示window對象中的方法" onclick="windowMove()">-->
	<input type="button" value="演示window對象中的方法" onclick="windowOpen()">
	<input type="button" value="停止演示" onclick="stopTime()">
</body>
</html>
```
