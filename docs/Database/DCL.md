# DCL

## 概述
- 一個項目創建一個用戶，一個項目對應的數據庫只有一個。
- 這個用戶只能對這個數據庫有全縣，其他數據庫你操作不了。

## 創建用戶
- CREATE USER 用戶名@IP地址 IDENTIFIED BY '密碼';
    - 用戶只能再指定的IP地址上登錄
    
- CREATE USER 用戶名@'%' IDENTIFIED BY '密碼';
    - 用戶可以再任意IP地址上登錄

## 給用戶授權
- GRANT 權限1, ..., 權限n ON 數據庫.* TO 用戶名@IP地址
    - 權限、用戶、數據庫
    - 給用戶分派再指定的數據庫上的指定的權限。
    - 例如: GRANT CREATE,ALTER, DROP,INSERT,UPDATE,DELETE,SELECT ON mydb1.* TO user1@localhost;
        - 給 user1 用戶分派再 mydb1 數據庫上的 create, alter, drop, insert, update, delete, select 權限
    - GRANT ALL ON 數據庫.* TO 用戶名@IP地址;
        - 給用戶分派指定數據庫上的所有全縣

## 撤銷授權
- REVOKE 權限1, ..., 權限n ON 數據庫.* FROM 用戶名@IP地址;
    - 撤銷指定用戶再指定數據庫上的指定權限
    - 例如: REVOKE CREATE, ALTER, DROP ON mydb1.* FROM user1@localhost;
        - 撤銷 user1 用戶再 mydb1 數據庫上的 create, alter, drop 權限

## 查看權限
- SHOW GRANTS FOR 用戶名@IP地址
    - 查看指定用戶的權限

## 刪除用戶
- DROP USER 用戶名@IP地址
                                            
