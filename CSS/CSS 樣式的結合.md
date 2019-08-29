# CSS 樣式的結合

## 演示
```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style type="text/css">
		ul {
			list-style-type: cambodian; 
		}
		
		table {
			border-bottom: double;
			border-top: dotted;
			border-right: #FA0606;
			border-left: #00FD3D;
			width: 500px;
		}
		
		table td {
			border:#0C35F0;
			padding: 20px;
		}
		
		div {
			border: #FF001F dashed 2px;
			height: 200px;
			width: 500px;
		}

		input {
			border: none;
			border-bottom: #FC0404 2px solid; 
		}

		.jack {
			border: none;
			border-bottom: #0B778D 1px solid;
 		}
	</style>
</head>
<body>
	姓名:<input type="text">
	<hr>
	<div>
		div 區域
	</div>
	<ul>
		<li>無序項目列表</li>
		<li>無序項目列表</li>
		<li>無序項目列表</li>
	</ul>
	<table>
		<tr>
			<td>請輸入:<input type="text" class="jack"></td>
			<td>請輸入:<input type="text" class="jack"></td>
		</tr>
		<tr>
			<td>單元格</td>
			<td>單元格</td>
		</tr>
	</table>
</body>
</html>
```
