# window 對象 - 事件

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
		/*
		onunload = function() {

			alert("onunload run");//在對象卸載前立即觸發
		}

		onload = function() { 

			alert("onload run");//在瀏覽器完成對象的裝載後立即觸發
		}

		onbeforeunload = function() {

			alert("run");//在頁面將要被卸載前觸發
		}
		*/

		onload = function() {

			window.status = "Hello World";//顯示於狀態欄中
		}
	</script>
</body>
</html>
```
