# Debug

## Debug 調適模式(斷點調適模式)
- 使用這種模式，調適程序。(看到程序裡面數據的變化)
- **步驟:**
   - 設置斷點(讓程序運行停止在這一行)。
   - 使用 debug as 方式運行程序。
   - 在斷點那一行，有一綠色條，表示程序停止在這一行。
   - 可以讓程序向下執行
    - 使用 Step Over 單步執行(F6)。
    - Resume(F8): 表示調適結束，執行直接向下運行。(如之後還有斷點，則跳到下一個斷點，如果沒有則直接運行到結束)
- Debug 另外一個用途: 
   - 可以查看程序的**源代碼**。
   - F5 Step Into:  進入到方法。
   - Step Return: 返回。