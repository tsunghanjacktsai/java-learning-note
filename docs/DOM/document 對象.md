# document 對象

## 使用
- 該將標記型文檔進行**封裝**。
- 該對象的作用，是對可以標記型文檔進行操作。
  最常見的操作就是，想要實現動態效果，需要對節點操作，那麼要先獲取到這個節點。
  要想獲取節點，必須要先獲取到節點所屬的文檔對象 document。
  所以 document 對象最常見的操作就是**獲取頁面中的節點**。
- 節點有三個必備屬性: 節點名稱、節點類型、節點值。
- 常見節點有三種: 
標籤型節點
屬性節點
文本節點
**注意:** 標籤型節點是沒有值的，屬性和文本節點是可以有值的。
- 獲取節點的方法體現:
1. getElementByID(): 通過標籤的 id 屬性值獲取該標籤節點，返回該標籤節點。
2. getElementsByName(): 通過標籤的 name 屬性獲取節點，因為 name 相同，所以返回一個數組。
3. getElementsByTagName(): 通過標籤名獲取節點。因為標籤名會**重複**，所以返回的是一個數組。
**注意:** 凡是帶 s 的返回的都是**數組**。


## 演示
**需求:** 獲取頁面中的 div 節點。
**思路:** 通過 document 對象完成。因為 div 節點有 id 屬性，所以可以通過id屬性來完成獲取。
```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<script type="text/javascript">
		function getNodeDemo() {

			var divNode = document.getElementById("divid");

			//節點有三個必備屬性: 節點名稱、節點類型、節點值
			// alert(divNode.nodeName + " @ " + divNode.nodeType + " @ " + divNode.nodeValue);//DIV @ 1 @ null

			//獲取 div 節點中的文本
			var text = divNode.innerHTML;		
			// alert(text);//這是一個div區域

			//改變 div 中的節點
			divNode.innerHTML = "It has been changed";//It has been changed

			
		}

		//獲取文本框節點演示 getElementsByName 方法
			function getNodeDemo2() {

				var node = document.getElementsByName("user");
				// var node = document.getElementsByName("user")[0];
				
				// alert(node);//[object NodeList]
				// alert(node[0].nodeName);//INPUT
				alert(node[0].value);//顯示文本框裡輸入的值

			}

			//獲取超鏈接節點對象，演示 getElementsByTagName
			function getNodeDemo3() {

				//直接用標籤名獲取
				var nodes = document.getElementsByTagName("a");

				// alert(nodes.length);//1
				// alert(nodes[0].innerHTML);//百度
				for(var x = 0; x < nodes.length; x++) {

					// alert(nodes[0].innerHTML);
					// nodes[x].target = "_blank";
				}
			}

			/*
			 * 對於頁面的超鏈接，Google 通過新窗口打開，門戶網站鏈接在當前頁面打開
			 * 通過 document 拿到的是	頁面中所有的超鏈接節點。
			 * 但是想要只獲取一部分，只要獲取被操作的超鏈接所屬的對象即可。
			 * 再通過這個節點獲取到它裡面所有的超鏈接節點。
			 */
			 function getNodeDemo4() {

			 	//獲取超鏈接所屬的 div 節點
			 	var divNode = document.getElementById("googleLink");

			 	//通過對 div 對象方法的查找發現它也具備 getElementsByTagName 方法
			 	//注意: 所有的容器型標籤都具備這個方法。再該標籤範圍內獲取指定名稱的標籤
			 	var aNodes = divNode.getElementsByTagName("a");
			 	for(var x = 0; x < aNodes.length; x++) {

			 		// alert(aNodes[x].innerHTML);
			 		aNodes[x].target="_blank";
			 	}
			 }
	</script>
	<!--<input type="button" value="演示document對象獲取節點" onclick="getNodeDemo()">-->
	<!-- <input type="button" value="演示document對象獲取節點" onclick="getNodeDemo2()"> -->
	<!--<input type="button" value="演示document對象獲取節點" onclick="getNodeDemo3()">-->
	<input type="button" value="演示document對象獲取節點" onclick="getNodeDemo4()">
	<div id="divid">這是一個div區域</div>
	<input type="text" name="user">
	<a href="https://www.baidu.com">百度1</a>
	<a href="https://www.baidu.com">百度2</a>
	<a href="https://www.baidu.com">百度3</a>
	<div id="googleLink">
		<a href="https://www.google.com">Google1</a>
		<a href="https://www.google.com">Google2</a>
		<a href="https://www.google.com">Google3</a>
	</div>
</body>
</html>
```
