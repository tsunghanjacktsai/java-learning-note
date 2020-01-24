# window 對象 - 對象

## 定義
- Browser Object Model 瀏覽器對象模型。
- 這個模型方便於**操作瀏覽器**。
  瀏覽器對應的對象就是 window 對象，這個可以通過查閱 dhtml api 獲得。
- 演示: 定義一個事件源，通過對事件源的觸發，獲取想要的結果。比如讓用戶通過點擊按鈕就可以知道瀏覽器的一些信息。

## window 對象
- navigator 對象: 想要知道瀏覽器的信息，就需要使用 window 對象中的 navigator。
- location 對象: window 對象中用於獲得當前頁面的地址(URL)，並把瀏覽器重定向到新的頁面。

## 演示
```javascript
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
	<!--定義按鈕 onclick 事件的處理方式-->
	<script type="text/javascript">
		//定義函數
		function windowObjDemo() {

			// alert("Window Run");

			//想要知道瀏覽器的信息，就需要使用 window 對象中的 navigator
			var name = window.navigator.appName;
			var version = window.navigator.appVersion;

			println(name + " @ " + version);
			// Netscape @ 5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/64.0.3282.140 Safari/537.36

		}

		function windowObjDemo2() {

			// var pro = location.protocol;//file:
			// var text = location.href;//file:///C:/Users/asus/Desktop/%E7%B6%B2%E9%A0%81/40.%20DOM%20-%20BOM.html
			// alert(text);

			//給 location 的 href 屬性設置一個值，改變地址欄的值，並對其值進行解析
			//如果是 http，則進行連接訪問
			location.href = "https://www.baidu.com";
		}
	</script>
	<!--定義事件源，註冊事件關聯的動作-->
	<!-- <input type="button" value="演示window中的對象" onclick="windowObjDemo()"> -->
	<input type="button" value="演示window中的對象" onclick="windowObjDemo2()">
</body>
</html>
```
