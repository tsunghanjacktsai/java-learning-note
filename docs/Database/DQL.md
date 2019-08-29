# DQL

## 概述
數據查詢語言，查詢不會修改數據庫表紀錄。

## 字段(列)控制
- 查詢所有列
SELECT * FROM 表名;
SELECT * FROM emp;
-> 其中"*"表示查詢所有列

- 查詢指定列
SELECT 列1 [, 列2, ... 列n] FROM 表名;
SELECT empno, ename, sal, comm FROM 表名;

- 完全重複的紀錄只一次
  當查詢結果中的多航紀錄一模一樣時，只顯示一行。一班查詢所有列時很少會有這種情況，但只查詢一列(或幾列)時，這種可能就很大。
  SELECT DISTINCT * | 列1 [, 列2, ... 列n] FROM 表名;
  SELECT DISTINCT sal FROM emp;
  -> 查詢員工表的工資，如果存在相同的工資則只顯示**一次**。

- 列運算
    - 數量類型的列可以做加、減、乘、除運算
      SELECT sal*1.5 FROM emp;
      SELECT sal+comm FROM emp;
    - 字符串類型可以做連續運算
      SELECT CONCAT('$', sal) FROM emp;
    - 轉換 NULL 值
      有時需要把 NULL 轉換成其他值，例如 com+1000 時，如果 com 列存在 NULL 值，那麼 NULL+1000 還是 NULL, 而我們這時希望把 NULL 當成 0 來運算。
      SELECT INNULL(comm, 0)+1000 FROM emp;
      -> IFNULL(comm, 0): 如果 comm 中存在 NULL 值，那麼當成 0 來運算。
    - 給列起別名
      當使用列運算後，查詢出的結果集中的列名稱不好看，這時我們需要給列名起個別名，這樣再結果集中列名就顯示的是別名-> 其中 AS 可以省略

## 條件控制
- 條件查詢
  與前面介紹的 UPDATE & DELETE 語句一樣，SELECT 語句也可以使用 WHERE 紫劇來控制紀錄。
    - SELECT num, name, sal, comm FROM emp WHERE sal > 10000 AND comm IS NOT NULL;
    - SELECT num, name, sal FROM emp WHERE sal BETWEEN 20000 AND 30000;
    - SELECT num, name, job, FROM emp WHERE job IN('經理'，'董事長');
- 模糊查詢
  當你想查詢姓張，並且姓名一共兩個字的員工時，這時就可以使用模糊查詢
    - SELECT * FROM emp WHERE name LIKE '張';
      -> 模糊查詢需要使用運算符: LIKE, 其中匹配一個任意字符，只匹配一個字符而不是多個。
      -> 上面語句查詢的是姓張，名字由兩個字組成的員工。
    - SELECT * FROM emp WHERE name LIKE '___'; //姓名由三個字組成的員工
  如果我們想查詢姓張，名字幾個字都可以的員工時就要使用"%"了。
    - SELECT * FROM emp WHERE name LIKE '張%';
      -> 其中% 匹配 0~n 個任意字符，所以上面語句查詢的是姓張的所有員工。
    - SELECT * FROM emp WHERE name LIKE '%張%';
      -> 千萬不要認為上面語句是再查詢姓名中間有張字的員工，因為%匹配0~n個字符，所以姓名以張開頭和結尾的員工也都會查詢到。
    - SELECT * FROM emp WHERE name LIKE'%';
      -> 這個條件等同於不存在，但如果姓名為 NULL 的查詢不出來。

## 排序
- 升序
    - SELECT * FROM emp ORDER BY sal ASC;
      -> 按 sal 排序，升序。
      -> 其中 ASC 是可以省略的。
- 降序
    - SELECT * FROM emp ORDER BY comm DESC;
      -> 按 comm 排序，降序。
      -> 其中 DESC 不能省略。
- 使用多列作為排序條件
    - SELECT * FROM WHERE emp ORDER BY sal ASC, comm DESC;
      -> 使用 sal 升序排，如果 sal 相同時，使用 comm 的降序排。

## 聚合函數
聚合函數用來做某列的縱向運算。
- COUNT
  SELECT COUNT(*) FROM emp;
  -> 計算 emp 表中所有列都不為 NULL 的紀錄的行數。
  SELECT COUNT(comm) FROM EMP;
  -> 計算 emp 表中 comm 列不為 NULL 的紀錄的行數。
- MAX
  SELECT MAX(sal) FROM emp;
  -> 查詢最高工資。
- MIN
  SELECT MIN(sal) FROM emp;
  -> 查詢最低工資。
- SUM
  SELECT SUM(sal) FROM emp;
  -> 查詢工資和。
- AVG
  SELECT AVG(sal) FROM emp;
  -> 查詢平均工資。

## 分組查詢
分組查詢是把紀錄使用某一列進行分組，然後查詢組信息。
例如: 查看所有部門的紀錄數。

- SELECT deptno, COUNT(*) FROM emp GROUP BY deptno;
  -> 使用 deptno 分組，查詢部門編號和每個部門的紀錄數。
- SELECT job, MAX(sal) FROM emp GROUP BY job;
  -> 使用 job 分組，查詢每種工作的最高工資。

**組條件**
以部門分組，查詢每組紀錄數。條件紀錄數大於 3

- SELECT deptno, COUNT(*) 
  FROM emp GROUP BY deptno HAVING COUNT(*) > 3

## LIMIT 子句(方言)
LIMIT 用來限定查詢結果的起始行，以及總行數。
例如: 查詢起始行為第5行，一共查詢3行紀錄

- SELECT * FROM emp LIMIT 4, 3;
  -> 其中4表示從第5行開始，其中3表示一共查詢3行。即第5, 6, 7 行紀錄。


1. 一頁的紀錄數: 10 行。
2. 查詢**第3頁**

- SELECT * FROM emp LIMIT 20, 10; //前2頁分別有10行，從0開始
    - (當前頁-1) * 每頁紀錄數
    - (3-1)_ * 10
    

## 語句執行順序
1. SELECT
2. FROM
3. WHERE
4. GROUP BY
5. HAVING
6. ORDER BY
