# DDL

## 數據庫
- 查看所有數據庫: SHOW DATABASES
- 切換 (選擇要操作的) 數據庫: USE 數據庫名
- 創建數據庫: CREATE DATABASE [IF NOT EXISTS] mydb [CHARSET=utf8]
- 刪除數據庫: DROP DATABASE [IF EXISTS] mydb1
- 修改數據庫編碼: ALTER DATABASE mydb1 CHARACTER SET utf8

## 數據類型(列類型)
- int: 整型
- double: 浮點型，例如double(5, 2) 表示最多5位，其中必須有2位小數，即最大值為999.99
- decimal: 浮點型，不會出現精度缺失問題。(常用於表示錢)
- char: **固定長度**字符串類型: char(255) -> 較 varchar 節省空間，不需要另外拉出空間計算長度。
- varchar: **可變長度**字符串類型: varchar(65535), jack -> 比 char 的優勢為因為是可變長度，所以後面不需要補0。
- text(clob): 字符串類型
    - 很小: tinytext
    - 小: text
    - 中: mediumtext
    - 大: longtext
- blob: 字節類型:
    - 很小: tinyblob
    - 小: blob
    - 中: mediumblob
    - 大: longblob
- data: 日期類型，格式為: YYYY-MM-DD
- time: 時間類型，格式為: HH-MM-SS
- timestamp: 時間戳類型

## 表
- 創建表:
```SQL
CREATE TABLE [IF NOT EXISTS] 表名 (
    列名 列類型，
    列名 列類型，
    ...
    列名 列類型
);
```
- 查看當前數據庫中所有表名稱: SHOW TABLES;
- 查看表結構: DESC 表名;
- 刪除表: DROP TABLE 表名:
- 修改表: ALTER TABLE 表名
    - 修改之添加列:
        ```SQL
        ALTER TABLE 表明 ADD (
            列名 列類型，
            列名 列類型，
            ...
        )
        ```
    - 修改之修改列類型(如果被修改的列已存在數據，那麼新的類型可能會影響到已存在數據): ALTER TABLE 表名 MODIFY 列名 列類型;
    - 修改之修改列名: ALTER TABLE 表明 CHANGE 原列名 新列名 列類型;
    - 修改之刪除列: ALTER TABLE 表名 DROP 列名;
    - 修改表名稱: ALTER TABLE 原表名 RENAME TO 新表名;
