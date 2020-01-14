# JS 綜合練習

1. 定義功能，完成對數組的最值獲取。
2. 對數組排序。
3. 對數組查找。
4. 對數組元素進行反轉。

## 演示
```javascript
    <script type="text/javascript">

		var arr = [1, 4, 56, 33, 20, 45]

		function println(val) {
			
			document.write(val + "<br>");
		}

		//取最值
		function getMax(arr) {

			var max = 0;

			for(var x = 1; x < arr.length; x++) {

				if(arr[x] > arr[max]) {
					max = x;
				}
			}

			return arr[max];
		}

		var maxValue = getMax(arr);
		// alert("max = " + maxValue);

		//排序
		function sortArray(arr) {

			for(var x = 0; x < arr.length; x++) {
				for(var y = x + 1; y < arr.length; y++) {
					if(arr[x] > arr[y]) {
						swap(arr, x, y);
					}
				}
			}
		}

		function swap(arr, x, y) {
			var temp = arr[x];
			arr[x] = arr[y];
			arr[y] = temp;
		}

		// println("Before: " + arr);
		// sortArray(arr);
		// println("After: " + arr);

		//普通查找
		function searchArr(arr, key) {

			for(var x = 0; x < arr.length; x++) {
				if(arr[x] == key)
					return x
			}
			return -1;
		}

		//折半查找
		function binarySearch(arr, key) {

			var max, min, mid;
			min = 0
			max = arr.length - 1;

			while(min <= max) {
				mid = (max + min) / 2;

				if(key > arr[mid]) {
					min = mid + 1;
				} else if(key < arr[mid]) {
					max = mid - 1;
				} else {
					return mid;
				}
				return -1;
			}
		}

		var find = searchArr(arr, 33);
		// println(find);

		//數組反轉
		function reverseArr(arr) {
			for(var start = 0, end = arr.length - 1; start < end; start++, end--) {
				swap(arr, start, end);
			}
		}

		println("Before: " + arr);
		reverseArr(arr);
		println("After: " + arr);
	</script>
```
