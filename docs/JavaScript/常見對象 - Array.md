# 常見對象 - Array

## 使用
- concat 方法: 連接元素或數組。
- join 方法: 定義分隔符。
- pop 方法: 刪除最後一個元素，並返回該元素。
- shift 方法: 刪除第一個元素，並返回該元素。
- sort 方法: 排序。
- splice 方法: 從數組中移除一個或多個元素，且可以再索移除元素的位置上插入新元素，返回所移除的元素。
- unshift 方法: 將指定的元素插入數組開始位置並返回該數組。

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
		var arr = ["Jack", "Vivian", "Jervis", "Mike", "Jacky", "Kevin"];

		var arr2 = ["nba", "cba", "sbl", "hbl"];

		println(arr);
		println(arr2);
		println("<hr>");

		//concat 方法
		//在 arr 數組上連接一個元素，再連接 arr2 數組
		
		//將 Hello 作為新數組中的元素，將 arr2 數組中的元素也作為新數組中的元素
		var newArr = arr.concat("Hello", arr2);

		println(newArr);//Jack,Vivian,Jervis,Mike,Jacky,Kevin,Hello,nba,cba,sbl,hbl

		//join 方法
		println(arr.join("-"));//Jack-Vivian-Jervis-Mike-Jacky-Kevin

		//pop 方法
		println(arr.pop());//Kevin
		println(arr);//Jack,Vivian,Jervis,Mike,Jacky

		//shift 方法
		println(arr.shift());//Jack
		println(arr);//Vivian,Jervis,Mike,Jacky

		//splice 方法
		var temp = arr2.splice(1, 3, "nba", "hbl", "fifa");
		println(temp);
		println(arr2);//nba,nba,hbl,fifa

		//unshift 方法
		println(arr2.unshift("hahahaha"));//5
		println(arr2);//hahahaha,nba,nba,hbl,fifa
	</script>
</body>
</html>
```
