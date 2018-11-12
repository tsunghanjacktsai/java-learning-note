# JS 數組

## 使用
數組用於存儲更多的數據，是一個容器。
- 特點:
1. 長度式可變的。
2. 元素的類型式任意的。
**注意:** 建議在使用數組時，存儲同一類型的元素，操作起來比較方便。
- JS 中的數組定義的兩種方式:
1. var arr = [ ]; var arr = [3, 2, 1];
2. 使用了 JS 中的 Array 對象來完成的定義
   var arr = new Array(); //var arr = [];
   var arr1 = new Array(5); //數組定義且長度是5
   var arr2 = new Array(5, 6, 7);//定義一個數組，元素是 5, 6, 7

## 演示
```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<script type="text/javascript">
		var arr = [1, 2, 3];

		// alert(typeof(arr));//對象類型 object

		// alert("len: " + arr.length);//len = 3

		//遍歷數組
		for(var x = 0; x < arr.length; x++) {
			document.write("arr[" + x + "] = " + arr[x] + "<br>");
		}
	</script>
</body>
</html>
```
