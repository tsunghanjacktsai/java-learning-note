# CSS 布局-漂浮

## float (漂浮)
- none: 默認值。對象不漂浮。
- left: 文本流向對象的右邊。(文本置左)
- right: 文本流向對象的左邊。(文本置右)

## clear
- none: 允許兩邊都可以有浮動對象。
- left: 不允許左邊有浮動對象。
- right: 不允許右邊有浮動對象。
- both: 不允許有浮動對象

## Example
```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style type="text/css">
		#div_1 {
			background-color:#080000;
			color: #FFFFFF;
			height: 100px;
			width: 200px;
			float:left;
		}
		
		#div_2 {
			background-color:#09E49F;
			height: 100px;
			width: 200px;
			margin: 100px;
		}
		
		#div_3 {
			background-color: #5E0F73;
			height: 100px;
			width: 200px;
			clear: left;
		}
	</style>
</head>
<body>
	<div id="div_1">盒子1</div>
	<div id="div_2">盒子2</div>
	<div id="div_3">盒子3</div>
</body>
</html>
```
