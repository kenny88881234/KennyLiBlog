---
title: 筆記 - MySQL 基礎
date: 2019-06-28 13:16:19
tags:
  - 筆記
  - 程式語言
  - SQL
  - 資料庫
  - MySQL
categories:
  - 筆記
keywords:
description:
top_img: /img/note.jpg
cover: /img/note.jpg
---
# [筆記] MySQL 基礎

### 基本介紹

#### 什麼是 Database

![](https://i.imgur.com/ZYaxYeK.png)

* DBMS 為向 Database 存取的訪問接口
* 通常稱 DBMS + Database 為 Database，EX : MySQL、Oracle、MongoDB...等等

#### SQL 與 NoSQL

* 區別在於有沒有使用 SQL 語言，NoSQL 的資料儲存可以不需要固定的表格模式
* SQL : MySQL、Oracle...等等
* NoSQL : MongoDB...等等

### 安裝

Window :

* 建議用 Installer 下載安裝 : [MySQL Installer 5.7.26](https://dev.mysql.com/downloads/windows/installer/5.7.html)

    **中間跳過的步驟默認就好**

    ![](https://i.imgur.com/wuWpNry.png)

    * 建議選擇 Developer Default

    ![](https://i.imgur.com/CcbMwYt.png)

    * 這步檢查依賴，`Execute` 就好

    ![](https://i.imgur.com/uPpWiDu.png)

    * 這邊設定密碼

    ![](https://i.imgur.com/IcsLgw7.png)

    * 這邊輸入剛剛設定的密碼，點擊 `Check`

    ![](https://i.imgur.com/twA25Yx.png)

    * 之後在這裡進行操作，輸入剛剛設定的密碼

**P.S.後來在另一台電腦一直無法安裝，有閃退的情況，後來安裝 8.0.16 就正常了**

macOS :

* 下載地址 : [MySQL 官網 - MySQL Community Server 5.7](https://dev.mysql.com/downloads/mysql/5.7.html#downloads)

### Database 與 Table

* Database 是由很多 Table 組成的
* Table 存放資料，且是有結構的

#### Database 基本操作

* `show databases;` : 顯示目前有哪些 Database
* `create database <name>;` : 創建 Database
* `drop database <name>;` : 刪除 Database
* `use <database name>;` : 切換 Database
* `select database();` : 查看目前正在使用的是哪個 Database
* `delimiter <結束符號>` : 預設結束符號是 `;`，這行命令可以做修改 ( 不建議 )
* `show warnings;` : 查看錯誤訊息

    **SQL 大小寫都可運行，建議關鍵字大寫**

#### Table

![](https://i.imgur.com/1I7Ew2d.png)

* column : 行，相同的資料類型
* row : 列，一筆一筆的數據

#### Table Data Type

有三種大分類 : Numeric、String、Date

* [資料類型官方文檔](https://dev.mysql.com/doc/refman/5.7/en/data-type-overview.html)

#### Table 的創建

```sql=
CREATE TABLE person
(
    name VARCHAR(20),
    phone VARCHAR(20),
    age INT
);
```

#### Table 基本操作

* `show tables` : 顯示目前有哪些 Table
* `show columns from <table name>` : 顯示 Table 的每個 columns
* `desc <table name>` : 描述 Table 的資訊 ( 顯示跟上面的命令一樣，可以用這條命令就好 )

    ![](https://i.imgur.com/sxToAEH.png)

* `drop table <table name>` : 刪除 Table

### Data Insert

#### 如何插入資料

```sql=
insert into <table_name>(<column1_name>, <column2_name>) 
values(<column1_data>, <column2_data>),
      (<column1_data>, <column2_data>);
```

#### 查看資料

```sql=
SELECT * from <table_name>;
```

* 查看 Table 裡的所有資料

```sql=
SELECT <column_name> from <table_name>;
```

* 查看 Table 裡某個 column 的所有資料

#### 創建特殊要求的 Table

* NULL 和 NOT NULL :

    * NULL 默認為 YES
    * 創建時 `<column name> <data type> NOT NULL`，NULL 就會變 NO
    * **插入資料時，NULL 為 NO 的資料不能為空**
    
* Default Value :

    * Default 默認為 NULL
    * 創建時 `<column name> DEFAULT <value>`

* PRIMARY KEY :

    * 唯一性，不能重複，不可 NULL
    * 創建時 `<column name> <data type> PRIMARY KEY`
    * 如果需要兩個以上 PRIMARY KEY，可以設置聯合 PRIMARY KEY
    * 聯合 PRIMARY KEY : `PRIMARY KEY(<column1 name>, <column2 name>)`

* UNIQUE :

    * 唯一性，不能重複，可 NULL
    * 創建時 `<column name> <data type> UNIQUE`

* AUTO_INCREMENT :

    * 設置時，必須同時是個 Key
    * 創建時 `<column name> <data type> <PRIMARY KEY or UNIQUE> AUTO_INCREMENT` ，常用在 ID 之類，不填值時會累加

### CRUD 基本的增刪改查

* Create
* Read
* Update
* Delete

#### 使用 SQL 文件 Create

* 創建一個使用 SQL 語法的文件 `test.sql`
* `source <SQL 文件路徑 EX : /Users/user/SQL-training/test.sql>;` 注意分隔要 `/`

```sql=
CREATE TABLE IF NOT EXISTS person
(
    name VARCHAR(20),
    phone VARCHAR(20),
    age INT
);
```

* `IF NOT EXISTS` 可以在創建 Table 時，查看 Table 是否存在，不存在就創建，存在則否

#### Read

```sql=
SELECT <column_name> as <別名> from <table_name>;
```

* column 的名稱會以別名顯示

```sql=
SELECT * from <table_name> WHERE <column1_name> = "想找到的資料" <AND 或 OR> <NOT> <column2_name> = "想找到的資料";
```

* WHERE 像是 if，可以過濾

#### Update

```sql=
UPDATE <table_name> set <column_name> = "想改成這個資料" WHERE ...;
```

* 通常會加上 WHERE 來過濾要改的資料
* 較安全步驟為

    1. 先 SELECT + WHERE 找出想改的資料
    2. 再來 UPDATE + 剛剛的 WHERE 來更動

#### Delete

```sql=
DELETE from <table_name> WHERE ...;
```

* 跟 UPDATE 一樣通常加上 WHERE 過濾
* 跟 UPDATE 一樣，先 SELECT + WHERE 較安全

### SQL 字串相關處理方法

#### 拼接 CONCAT

* CONCAT :

    ```sql=
    SELECT CONCAT(<column1_name>, <column2_name>) as <別名> from <table_name>;
    ```

    * 輸出會是 `<column1_name><column2_name>`

* CONCAT_WS :

    ```sql=
    SELECT CONCAT_WS("<間格符>", <column1_name>, <column2_name>, <column3_name>) as <別名> from <table_name>;
    ```

    * 輸出會是 `<column1_name><間格符><column2_name><間格符><column2_name>`

#### 剪輯 SUBSTRING、REPLACE

* SUBSTRING :

    * `SUBSTRING("Hello World", 7)` : 從前面數，輸出為 `World`
    * `SUBSTRING("Hello World", -3)` : 從後面數，輸出為 `rld`
    * `SUBSTRING("Hello World", 1, 4)` : 第 n 個到第 m 個，輸出為 `Hell`

* REPLACE :

    * `REPLACE("Hello World", "World", "MySQL")` : 替換，輸出為 `Hello MySQL`

#### 反轉 REVERSE

* `REVERSE("Hello World")` : 反轉，輸出為 `dlroW olleH`

#### 長度 CHAR_LENGTH

* `CHAR_LENGTH("Hello World")` : 字串長度，輸出為 `11`

#### 大小寫 LOWER、UPPER

* `LOWER("Hello World")` : 轉小寫
* `UPPER("Hello World")` : 轉大寫
* 數字符號都不會變

### SELECT 進階

#### 按規則排序 ORDER BY

```sql=
SELECT * from <table_name> ORDER BY <column_name>;
```

* 會按照 `column_name` 進行升序排序
* 如要進行降序排序 : `ORDER BY <column_name> desc`
* 字母、日期也可排序

```sql=
SELECT <column1_name>, <column2_name>, <column3_name> from <table_name> ORDER BY 3, 1;
```

* 按照 `column3_name` 與 `column1_name` 進行排序
* 如果 `column3_name` 相同才按照 `column1_name` 排序

#### 限制搜尋資料顯示的資料數目 LIMIT

```sql=
SELECT * from <table_name> ORDER BY <column_name> LIMIT 3;
```

* 顯示排序後前三個
* `LIMIT 2, 4` : 顯示從 2 開始後 4 筆資料
* `LIMIT 2, 18446744073709551615` : 顯示從 2 開始後的所有資料

#### 模糊搜尋 LIKE

```sql=
SELECT * from <table_name> WHERE <column_name> LIKE "%i%";
```

* 顯示包含 `i` 的
* `%` 代表任意字符
* `_` 代表一個任意字符
* 如果要找的資料包含 `%` 或 `_`，可以寫成 `\%` 和 `\_`
* SQL 裡搜尋是不區分大小寫的，如要區分需要額外設置

### 資料聚合處理

#### 計數 COUNT

```sql=
SELECT COUNT(*) from <table_name>;
```

* 計算資料數量

#### 統計唯一值 DISTINCT

```sql=
SELECT DISTINCT <column_name> from <table_name>;
```

* 將 `column_name` 內重複的資料去除後顯示出來

#### 資料整合 GROUP BY

```sql=
SELECT COUNT(<column_name>) from <table_name> GROUP BY <想合併的 column_name>;
```

* 將 `想合併的 column_name` 分組後算出每組 `column_name` 的資料數量

#### 最大值與最小值 MAX、MIN

* `MAX(<column_name>)` : `column_name` 的最大值
* `MIN(<column_name>)` : `column_name` 的最小值

#### 和與平均值 SUM、AVG

* `SUM(<column_name>)` : `column_name` 的和
* `AVG(<column_name>)` : `column_name` 的平均值

#### HAVING

`WHERE` 只能對原始資料過濾，假如要對 `GROUP BY` 之後的資料過濾就要用到 `HAVING`

```sql=
SELECT COUNT(<column_name>) from <table_name> GROUP BY <想合併的 column_name> HAVING <column1_name> = "想找到的資料";
```

### Data Type 詳細介紹

#### 數值類型

* 整數 ( Integer ) - 準確的

    * 標準類型 :

        * INTEGER ( INT )
        * SMALLINT

    * 擴充類型 :

        * TINYINT
        * MEDIUMINT
        * BIGINT

    * 後面加 `UNSIGNED` 表示為無負號

* 定點數 ( Fixed-Point ) - 準確的

    * DECIMAL ( 用於金錢 )

        * `DECIMAL(m, d)` : 

            * 總共 m 位，小數點後 d 位
            * 小數點後不足補 0，多的四捨五入
            * 可負數，不算位數

    * NUMERIC : 跟 DECIMAL 是一樣的

* 浮點數 ( Floating-Point ) - 不準確的

    * FLOAT

        * `FLOAT(m, d)` : 

            * 總共 m 位，小數點後 d 位

    * DOUBLE : 用法跟 FLOAT 是一樣的，差別在於 DOUBLE 可存字節較多

* 位 ( Bit-Value )

    * BIT

        * `BIT(m)` : 

            * 總共 m 位
            * m 最小 1，最大 64，默認為 1

        * 插入方法 :

            ```sql=
            insert into <table_name> values(b'1010')
            ```

            * 純數字為十進位
            * `b'1010'` 為二進位

        * 顯示方法 :

            ```sql=
            SELECT <column_name>+0 from <table_name>
            ```

            * `<column_name>+0` : 顯示十進位數值
            * `bin(<column_name>+0)` : 顯示二進位數值
            * `oct(<column_name>+0)` : 顯示八進位數值
            * `hex(<column_name>+0)` : 顯示十六進位數值

* [數值類型的官方文件](https://dev.mysql.com/doc/refman/5.7/en/numeric-types.html)

#### 時間類型

* DATE

    * 日期 ( YYYY-MM-DD )
    * 範圍 : `1000-01-01` ~ `9999-12-31`
    * 可插入 INT 型 `20181001`，也可不需 `-` `"20181001"`，但不可缺少年月日

* TIME

    * 時間 ( HH:MM:SS )
    * 範圍 : `-838:59:59` ~ `838:59:59`，小時範圍那麼大的原因是，也可以表示時間差
    * 插入規則同 DATE，但可缺少時分秒

        * `12:10` 為 `12:10:00`
        * `1210` 為 `00:12:10`
        * `10` 為 `00:00:10`

* YEAR

    * 年，為四個字符
    * 範圍 : `1901` ~ `2155`
    * 插入兩位 INT :

        * `1` ~ `69` 為 `2001` ~ `2069`
        * `70` ~ `99` 為 `1970` ~ `1999`
        * `0` 為 `0000`

    * 插入兩位 STRING :

        * `"0"` ~ `"69"` 為 `2000` ~ `2069`
        * `"70"` ~ `"99"` 為 `1970` ~ `1999`

* DATETIME

    * 日期 + 時間 ( YYYY-MM-DD HH:MM:SS )
    * 範圍 : `1000-01-01 00:00:00` ~ `9999-12-31 23:59:59`

* TIMESTAMP

    * 範圍 : `"1970-01-01 00:00:01" UTC` ~ `"2038-01-19 03:14:07" UTC`
    * 會隨著 timezone 改變
    * 默認當前時間
    * `SELECT NOW()` : 查看系統當前時間
    * 以下兩條程式碼是相同功能的

    ```sql=
    CREATE TABLE <table_name>(<column_name> TIMESTAMP)
    CREATE TABLE <table_name>(<column_name> TIMESTAMP DEFAULT NOW() ON UPDATE NOW())
    ```

* timezone

    * `show variables LIKE "%time_zone%"` : 查看包含 time_zone 字串的變量
    * `SET time_zone = "-12:00"` : 更改 time_zone，根據 UTC 時間減 12 小時
    * `SET time_zone = "system"` : 更改 time_zone 為系統當前時區
    * time_zone 也可用國家名稱下去設置

* [時間類型的官方文件](https://dev.mysql.com/doc/refman/5.7/en/date-and-time-type-overview.html)
* [時間與日期函式](https://dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html)

#### 字符串類型

* CHAR 與 VARCHAR

    * 後面帶的數字代表字串長度
    * CHAR : 適用於固定長度

        * 較不省空間

    * VARCHAR : 適用於不固定長度

        * 更新效率較慢，不如 CHAR
        * 平常會多 1 bytes 儲存字串長度

* BINARY 與 VARBINARY

    * 後面帶的數字代表 bytes 長度
    * BINARY : 適用於固定長度

        * 較不省空間
        * 插入的字串不足會補上 `\0`

    * VARBINARY : 適用於不固定長度

        * 更新效率較慢，不如 BINARY

* BLOB 與 TEXT

    * 適用於儲存文章
    * BLOB : 較大的 BINARY
    * TEXT : 較大的 CHAR
    * `SET max_sort_length = n` : 設置此變量後，排序時只對前 n 個字符排序

* ENUM

    * 給予特定字串選擇 `ENUM("F", "M")`
    * 類似單選

* SET

    * 跟 ENUM 很像，也是一個 list
    * 但 SET 可以選擇一個集合
    * 類似複選

* [字符串類型的官方文件](https://dev.mysql.com/doc/refman/5.7/en/string-types.html)

#### 對已經定義好的 Table ，更改 Column 的資料類型

```sql=
ALTER TABLE <table_name> MODIFY <column_name> <想更改成的資料類型>
```

* 禁止 varchar 改成 int
* 修改資料類型前一定要備份

### SQL 邏輯操作符

#### EQUAL 與 NOT EQUAL

* EQUAL : `<column_name> = 3000`
* NOT EQUAL : `<column_name> != 3000` 或 `NOT <column_name> = 3000`

#### LIKE 與 NOT LIKE

* 模糊搜尋

#### GREATER THAN 與 LESS THAN

* GREATER THAN : `<column_name> > 3000`
* LESS THAN : `<column_name> < 3000`
* 包含 : `>=`、`<=`

#### AND 與 OR

* AND : 條件都要滿足
* OR : 條件只要滿足一項

#### BETWEEN

* `<column_name> BETWEEN 1000 and 3000`

#### IN 與 NOT IN

* 搜尋 list 裡面有的值
* IN : `<column_name> IN (5000, 6000, 7000, 8000)`
* NOT IN : ``<column_name> NOT IN (5000, 6000, 7000, 8000)``

#### CASE STATEMENT

```sql=
SELECT <column_name>, 
    case
        when <column_name> >= 7000 then "high"
        else "low"
    end as <別名>
from <table_name>;
```

### MySQL 內建函式

#### 數值處理函式

* CEIL
* FLOOR
* DIV : 整數除法
* MOD
* POWER
* ROUND :四捨五入

#### 日期時間函式

* NOW
* CURDATE : 當前日期
* CURTIME : 當前時間
* DATE_FORMAT
* DATE_ADD : 時間加減
* DATEDIFF : 求時間差

#### 字串處理函式

* CONCAT
* CONCAT_WS
* LOWER
* UPPER
* LEFT : 擷取頭
* RIGHT : 擷取尾
* LENGTH
* LTRIM : 去頭
* RTRIM : 去尾
* TRIM : 去頭去尾
* REPLACE
* SUBSTRING

#### 信息函式

* CONNECTION_ID : 當前與 MySQL 連線的 ID
* DATABASE : 當前使用的 database
* LAST_INSERT_ID : 最後插入資料的資料 ID
* USER : 當前用戶
* VERSION

#### 聚合函式

* AVG
* COUNT
* MAX
* MIN
* SUM

#### 加密函式

* MD5 : 用來存取密碼
* PASSSWORD : 通常用於修改 MySQL 密碼 `SET PASSSWORD = PASSSWORD('ABC123')`

### 關聯 ( 一對多 )

#### 通過 ID 關聯兩個 Table

```sql=
CREATE TABLE customers(
    id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    email VARCHAR(100) 
);

INSERT INTO customers(first_name, last_name, email) VALUES
    ('Robin', 'Jackman', 'roj@gmail.com'),
    ('Taylor', 'Edward', 'taed@gmail.com'),
    ('Vivian', 'Dickens', 'vidi@gmail.com'),
    ('Harley', 'Gilbert', 'hgi@gmail.com');

CREATE TABLE orders(
    id INT AUTO_INCREMENT PRIMARY KEY,
    order_date DATE,
    amount DECIMAL(8,2),
    customer_id INT
);

INSERT INTO orders(order_date, amount, customer_id) VALUES
    ('2001-10-12', 99.12, 1),
    ('2001-09-21', 110.99, 2),
    ('2001-10-13', 12.19, 1),
    ('2001-11-29', 88.09, 3),
    ('2001-11-11', 205.01, 4);
```

* 這邊有兩張 Table，客戶訊息與訂單訊息，訂單訊息以 `customer_id` 與客戶訊息的 `id` 做關聯

```sql=
SELECT * from orders WHERE customer_id = (SELECT id from customers WHERE email = "roj@gmail.com");
```

* 查找 Robin Jackman 的訂單

#### 使用 FOREIGN KEY 約束關聯字段

```sql=
FOREIGN KEY (customer_id) REFERRENCES customers(id)
```

* 在創建 Table 時，加上這段可以約束訂單訊息的 `customer_id` 與客戶訊息的 `id`

#### 關聯表的操作

**使用 JOIN 合併關聯表**

![](https://i.imgur.com/VgX1FUb.png)

* INNER JOIN : 顯示出有關聯的所有資料 ( 沒關聯的不會顯示 )

```sql=
SELECT * from customers INNER JOIN orders WHERE customers.id = orders.customer_id;
```

`WHERE` 可改成 `ON`

* LEFT JOIN : 顯示出所有 SELECT 的 Table 以及他們的關聯資料

```sql=
SELECT * from customers LEFT JOIN orders WHERE customers.id = orders.customer_id;
```

`IFNULL(SUM(amount), 0)` : 如果值為 NULL 則顯示 0

* RIGHT JOIN : 顯示出所有 JOIN 的 Table 以及他們的關聯資料

```sql=
SELECT * from customers RIGHT JOIN orders WHERE customers.id = orders.customer_id;
```

* ON DELETE : 在刪除被參照的 Table 內資料時，也一起刪除其他 Table 參照此 Table 的相關資料

```sql=
ON DELETE CASCADE
```

* 在創建 Table 時，除了 FOREIGN KEY，另外加上這段

### 關聯 ( 多對多 )

**兩張多對多的 Table 會有一張 Table 把他們關聯起來**

![](https://i.imgur.com/EbgXtpe.png)

* books 與 reviewer 為多對多

### MySQL 存入中文，編碼問題

#### Window

* 指令同下
* 但因為 `MySQL 5.7 Command Line Client` 不支援中文，所以必須以其他方式插入中文資料
* `MySQL Workbench`、`VS code` 都可以

#### Mac

```sql=
CREATE database <database_name> DEFAULT CHARSET = utf8 COLLATE = utf8_general_ci;
```

* 創建資料庫時修改默認編碼

```sql=
ALTER database <database_name> CHARSET = utf8 COLLATE = utf8_general_ci;
```

* 已存在的 database 修改默認編碼

```sql=
ALTER table <table_name> CONVERT TO CHARACTER SET utf8;
```

* 若是修改了已存在的 database，其本身已存在 table，必須一併更改默認編碼

###### tags: `筆記` `程式語言` `SQL` `資料庫` `MySQL`