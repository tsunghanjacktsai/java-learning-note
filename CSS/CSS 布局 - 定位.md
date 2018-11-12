# CSS 布局 - 定位

## position (定位)
- static: 默認值。無特殊定位。
- absolute: 將對象從文檔流中脫出，使用 left, right, top, bottom 等屬性相對於其最接近的父對象進行絕對定位。
  沒定義過父對象則是相對於 body 部分。
- relative: 對象不可層疊，但將依據 left, right, top, bottom 等屬性在正常文檔流中偏移位置。

## Example
```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style type="text/css">
		div {
			height: 100px;
			width: 250px;
			color: #FFFFFF;
			margin:50px;
			border: solid 2px;
			border-color: #FC0303;
		}
		
		#div_0 {
			background-color: #0F33D3;
			height: 400px;
			width: 400px;
			position: absolute;
			top:100px;
			left:100px;
		}

		#div_1 {
			background-color:#080000;
			position: absolute;
			top: 70px;
			left: 90px;
		}
		
		#div_2 {
			background-color:#09E49F;
			position: relative;
			top: 100px;
		}
		
		#div_3 {
			background-color: #5E0F73;
		}
	</style>
</head>
<body>
	<div id="div_0"></div>
	<div id="div_1">盒子1</div>
	<div id="div_2">盒子2</div>
	<div id="div_3">盒子3</div>
</body>
</html>
```
