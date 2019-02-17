# MVC設計模式
## 流程圖
![](https://github.com/jack870131/Markdown-Pic/blob/master/Picture/mvc.png)

## 概述
MVC 設計模式不是 java 獨有，所有的 B/S 結構的項目都在使用他。
M -> Model 模型
V -> View 視圖
C -> Controller 控制器

- JSP: 視圖層，用來與用戶打交道。負責接收用戶的數據，以及顯示數據給用戶。
- Servlet: 控制曾，負責找到合適的模型對象來處以業務邏輯，轉發到合適的視圖。
- JavaBean: 模型層，完成具體的業務工作，例如: 開啟、轉帳等。
![](https://github.com/jack870131/Markdown-Pic/blob/master/Picture/mvc2.png)
