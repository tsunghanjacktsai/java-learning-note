# 常見對象 - Date

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
		var date = new Date();
		// println(date);//Fri Feb 09 2018 17:35:27 GMT+0800 (中国标准时间)
		// println(date.toLocaleString());//2018/2/9 下午5:37:37 -> 日期和時間
		// println(date.toLocaleDateString());//2018/2/9 -> 日期

		var year = date.getFullYear();
		var month = date.getMonth() + 1;
		var day = date.getDate();
		var week = getWeek(date.getDay());

		println(year + "-" + month + "-" + day + "-" + week);//2018-2-9-Fri

		function getWeek(num) {

			var weeks = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];

			return weeks[num];
		}

		//日期對象和毫秒值之間的轉換
		var date2 = new Date();
		//獲取毫秒值。 日期對象 --> 毫秒值
		var time = date2.getTime();
		println("time: " + time);//time: 1518170193507
		//將毫秒值轉成日期對象
		var date3 = new Date(time);

		//將日期對象和字符串之間進行轉換
		//日期對象轉成字符串 --> toLocaleString toLocaleDateString
		//將字符串轉成日期對象。具備指定格格式的日期字符串 --> 毫秒值 --> 日期對象
		var str_date = "2/9/2018";

		var time2 = Date.parse(str_date);

		var date3 = new Date(time2);

		println(date3);//Fri Feb 09 2018 00:00:00 GMT+0800 (中国标准时间)
	</script>
</body>
</html>
```
##with 語句
- 為了簡化對象調用內容的書寫，
可以使用 JS 中的特有語句 with 來完成。
- 格式:
with(對象)
{
    在該區域中可以直接使用指定的對象內容，不需要對象。
}
```
// var year = date.getFullYear();
// var month = date.getMonth() + 1;
// var day = date.getDate();
// var week = getWeek(date.getDay());
    
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<script type="text/javascript" src="JS/out.js"></script>
	<script type="text/javascript">
		/*
		var year = date.getFullYear();
		var month = date.getMonth() + 1;
		var day = date.getDate();
		var week = getWeek(date.getDay());
		*/
		var date = new Date();

		with(date) {

			var year = getFullYear();
			var month = getMonth() + 1;
			var day = getDate();
			var week = getWeek(getDay());

			println(year + "--" + month + "--" + day + "--" + week);

			function getWeek(num) {

			var weeks = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];

			return weeks[num];
			}
		}
	</script>
</body>
</html>
```
