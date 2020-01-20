# JS 自訂義對象

## 使用
- 如果想要自訂義對象，應該先對對象進行描述。
  JS 是**基於對象**，不是面向對象的。不具備描述事物的能力。
- 如果想以**面向對象**的思想編寫 JS，
  就要先描述，在 JS 中，可以用**函數**來模擬面向對象中的描述。

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
	<script type="text/javascript">
		//第一種方法
		/*
		//用 JS 來描述人
		function Person() {//相當於構造器
			alert("Person run");
		}

		//通過描述進行對象的建立 -> new
		var p = new Person();

		//動態給 p 對象添加屬性，直接使用 p.屬性名 即可
		p.name("Jack");
		p.age = 20;

		//如果定義 p 對象的屬性賦值為一個函數，即是給 p 添加一個方法
		p.show = function() {
			alert("show: " + this.name + " @ " + this.age);
		}

		p.show();
		*/

		//第二種方法
		/*
		function Person(name, age) {
			//在給 Person 對象中添加兩個屬性
			this.name = name;
			this.age = age;

			this.setName = function(name) {
				this.name = name;
			}

			this.getName = function() {
				return this.name;
			}
		}

		var p = new Person("Jack", 20);

		alert(p.name);
		p.setName("Mike");
		alert(p.getName());
		*/

		//第三種方法
		//直接使用 {} 定義屬性和值的鍵值對方式。鍵值對通過 : 連接，鍵與鍵之間用逗號隔開
		/*
		var pp = {
			"name": "Jack", "age": 20,
			"getName": function() {
				return this.name;
			}
		}

		//對象調用成員有兩種方式: 對象.屬姓名 對象["屬性名"](對象["鍵"] == 值)
		// alert(pp.age + " @ " + pp.getName());
		// alert(pp["age"] + " @ " + pp["name"]);

		//用 JS 實現鍵值對映射關係的集合容器
		var oMap = {
			8: "Jack", 3: "Vivian", 7: "Kevin"
		}

		var val2 = get(7);
		alert("val2: " + val2);

		function get(key) {
			return omap[key];
		}
		*/

		//鍵值對使用的幾種方式
		var myobj = {
			myname:"Jack", myage:20
		}

		// alert(myobj.myname + " @ " + myobj["myage"]);

		var myobj2 = {
			"myname2":"Mike", "myage2":80
		}
		
		// alert(myobj2.myname2 + " @ " + myobj2["myage2"]);

		var myMap = {
			// names:["Jack", "Vivian", "Mike"], nums:[54, 67, 20]

			names:[{name1:"Jack"}, {myname: "Jacky"}]
		}

		// alert(myMap.names[1]);//Vivian
		alert(myMap.names[0].name1);//Jack
	</script>
</body>
</html>
```
