# window 對象 - 練習

## 彈窗廣告
頁面一加載完成就執行。在當前頁面定義腳本，可以在 onload 事件中完成廣告的彈窗。

## 演示
```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<script type="text/javascript" src="JS/out.js"></script>
	<script type="text/javascript">
		onload = function() {

			open("https://www.baidu.com");
		}

		setInterval("focus()", 1000);//使彈跳窗口每隔一秒就顯示並前置窗口
	</script>
</body>
</html>
```
