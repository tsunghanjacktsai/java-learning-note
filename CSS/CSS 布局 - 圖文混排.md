# CSS 布局 - 圖文混排

## 演示
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>

	<style type="text/css">
		#imgtext {
			border: #000000 dashed 2px;
			width:200px;
		}

		#img {
			float: right;
		}

		#text {
			color: #08D6D8;
			font-family: Tahoma;
		}
	</style>
</head>
<body>
	<div id="imgtext">
		<div id="img">
			<img src="Image\1.jpg" height="100px" width="100px">
		</div>
		<div id="text">
			Hello World! My name is Jack! Nice to meet you!
			Hello World! My name is Jack! Nice to meet you!
			Hello World! My name is Jack! Nice to meet you!
		</div>
	</div>
</body>
</html>
```
