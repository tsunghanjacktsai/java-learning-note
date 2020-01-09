# JS 語句
## 語句
1. 順序結構
2. 判斷結構: if
3. 選擇結構: switch
4. 循環結構: while, do while, for
5. 其他語句: break: 跳出選擇，跳出循環
            continue: 用於循環語句，結束本次循環，繼續下次循環
```javascript
    //語句
	<script type="text/javascript">
	
	//if 語句
	var x = 3;
	
	/*
	//建議將常量放左邊，以報錯來修正代碼
	if(5 == x) {
		alert("yes");
	} else {
		alert("no");
	}//no
	*/

	/*
	//單等號為重新赋值，須注意
	if(x = 5) {
		alert("yes");
	} else {
		alert("no");
	}//yes
	*/

	/*
	if(x > 1) {
		alert("Jack");
	} else if(x > 2) {
		alert("Vivian");
	} else {
		alert("Mike");
	}//Jack -> 一個滿足即結束
	*/
		//switcj 語句
	/*
	var x = "abc";
	switch(x) {
		case "kk":
			alert("a");
			break;
		case "abc":
			alert("b");
			break;
		case "hh":
			alert("c");
			break;
		default:
			alert("d");
			// break;//可省略
	}
	*/
		//循環結構
	/*
	var x = 1;
	document.write("<font color = '#FD0202'>");
	while(x < 10) {
		// alert("x = " + x);
			//當數據直接寫到當前頁面當中
		document.write("x = " + x + "<br>");
		x++;
	}
	document.write("</font>")
	*/
	/*
	for(var x = 0; x < 3; x++) {
		document.write("x = " + x);
		}
		*/
	</script>
```

