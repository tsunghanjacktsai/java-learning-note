# JS 練習 - 九九乘法表

## 演示
```
//table.css
table, table td {
	border: #040000 double 1px;
	width: 800px;
}

//JS - 九九乘法表
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<link rel="stylesheet" type="text/css" href="CSS\table.css">
</head>
<body>
	<script type="text/javascript">
		document.write("<table>");
		for(var x = 1; x <= 9; x++) {
			document.write("<tr>");
			for(var y = 1; y <= x; y++) {
				document.write("<td>" + y + " * " + x + " = " + y * x + "</td>");
			}
			document.write("</tr>");
		}
		document.write("</table>");
	</script>
</body>
</html>
```
