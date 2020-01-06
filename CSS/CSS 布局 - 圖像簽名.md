# CSS 布局 - 圖像簽名

## 演示
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style type="text/css">
		#imgtext {
			background-color: #122F5A;
			border: #FFFFFF dotted 2px;
			width: 300px;
			position: absolute;
			top: 100px;
		}

		#img {
			
		}

		#text {
			color: #FFFFFF;
			font-family: Tahoma;
			position: absolute;
			top: 50px;
			left: 10px;
		}
	</style>
</head>
<body>
	<div id="imgtext">
		<div id="img">
			<img src="Image\1.jpg" height="250" width="200">
		</div>
		<div id="text">
			Hello World
		</div>
	<div>
</body>
</html>
```
