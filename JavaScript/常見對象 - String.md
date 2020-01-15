# 常見對象 - String

## 使用
- var str = new String("abc");
- var str = "abc";

## 自訂義功能
- 發現 JS 中的 String               對象方法有限，想要對字符串操作的其他功能。
  比如: 去除字符串兩端的空格。
- 將 trim 方法定義到字符串對象中 -> 直接用字符串對象調用 -> 使用一個該字符串的原型屬性來完成
- 原型: 該對象的一個描述。該描述中如果添加了新功能，那麼該對象都會具備這些新功能。
  而 prototype 就可以**獲取**到這個原型對象。
  通過 prototype 就可以對對象的功能進行**擴展**。
- Example: 虎的由來是貓，虎的原型是貓 - > 虎.prototype.爬樹的技能 = function() {}

## 演示
```javascript
//trim.js
//去除字符串兩端的空格
function trim(str) {

	//定義兩個變量，一個紀錄開始的位置，一個紀錄結束的位置
	//對開始的位置的字符進行判斷，如果是空格，就進行遞增，直到不是空格為止
	//對結束的位置的字符進行判斷，如果是空格，就進行遞減，直到不是空格為止
	//必需要保證開始 <= 結束，才可以進行擷取
	var start, end;
	start=0;
	end = str.length - 1;
		while(start <= end && str.charAt(start) == " ") {
		start++;
	}

	while(start <= end && str.charAt(end) == " ") {
		end--;
	}	
		return str.substring(start, end + 1);
}

<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>

</head>
<body>
	<script type="text/javascript" src="JS/out.js"></script>
	<script type="text/javascript" src="JS/trim.js"></script>
	<script type="text/javascript">
		/*
		var str = " abcde ";

		println(str);
		println(str.length);//字符串長度
		println(str.bold());//粗體
		println(str.fontcolor("#FC0505"));//字體顏色
		println(str.link("https://www.baidu.com"));
		println(str.substr(1, 3));//bcd
		println(str.substring(1, 3));//bc
		*/

		// println("-" + s + "-");//- hell o -
		// println("-" + trim(s) + "-");//-hell o-

		//需求: 想要給 String 對象添加一個可以去除字符串兩端空格的新功能 -> prototype

		//給 String 的原型中添加一個功能。 注意: 給對象添加新功能直接使用 對象.新內容 即可
		
		// String.prototype.len = 100;//給 string 的原型對象中添加一個屬性名為 len。 值為100。
		// println("jack".len);

		//添加行為
		String.prototype.trim = function() {

			var start, end;
			start=0;
			end = this.length - 1;

			while(start <= end && this.charAt(start) == " ") {
				start++;
			}
			while(start <= end && this.charAt(end) == " ") {
				end--;
			}	

			return this.substring(start, end + 1);
		}


		println("-" + "            ab cd           " + "-");//- ab cd -
		println("-" + "            ab cd           ".trim() + "-");//-ab cd-
	</script>
</body>
</html>
```
