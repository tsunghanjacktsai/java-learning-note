# DML

## 概述
數據操作語言，他是對表紀錄的操作(增、刪、改)

## 插入數據
- INSERT INTO 表名(列名1, 列名2, ...) VALUES(列值1, 列值2, ...);
    - 在表明後給出要插入的列名，其他沒有指定的列等同於插入 null 值。所以插入紀錄總是插入一行，不可能平行。
    - 在 VALUES 後給出列值，值得順序和個數必須與前面指定的列對應。
- INSERT INTO 表名 VALUES(列值1, 列值2);
    - 沒有給出要插入的列，那麼表示插入所有列。
    - 值得個數必須是該表列的個數。
    - 值得順序，必須與表創建時給出的列的順序相同。

## 修改數據
- UPDATE 表名 SET 列名1=列值1, 列名2=列值2, ... [WHERE 條件]
- 條件(條件可選):
    - 條件必須是一個 boolean 類型的值或表達式: UPDATE person SET gender="Male", age=age+1 WHERE sid='1';
    - 運算符: =、!=、&lt;&gt;、&gt;、&lt;、&lt;=、&gt;=、BETWEEN...AND、IN(...)、IS NULL、NOT、OR、AND

## 刪除數據
- DELETE FROM 表名 [WHERE 條件];
- TRUNCATE TABLE 表明: TRUNCATE 是 DDL 語句，他是先刪除 DROP 該表，再 CREATE 該表。而且**無法回滾**。
