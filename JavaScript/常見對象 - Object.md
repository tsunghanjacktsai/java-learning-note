# 常見對象 - Object

## 使用
1. toString(): 將對象變成字符串。
2. valueOf()

## 演示
```
<script type="text/javascript">
		function show() {
			alert("show run");
		}

		// alert(show.toString());

		var arr = [1, 2, 3, 6, 4, 8];

		// alert(arr.toString());//1, 2, 3, 6, 4, 8

		var abc = function() {
			alert("abc");
		}

		// alert(abc.toString());

		alert(arr.valueOf());//1, 2, 3, 6, 4, 8
	</script>
```


