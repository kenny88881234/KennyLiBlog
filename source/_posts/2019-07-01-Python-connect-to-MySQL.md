---
title: 實作 - Python 連結至 MySQL 並進行操作
date: 2019-07-01 15:20:32
tags:
  - 實作
  - Python
  - MySQL
categories:
  - 實作
keywords:
description:
top_img: /img/code.jpg
cover: /img/code.jpg
---
# [實作] Python 連結至 MySQL 並進行操作

### 安裝 MySQL 驅動

```=
$ pip install mysql-connector-python mysql-connector-python
```

* 在終端機輸入來安裝 MySQL 驅動

```python=
import mysql.connector
```

* 接著就能在 Python 引入

### 連結 MySQL

```python=
testdb = MySQL.connect(
    host = "localhost",
    user = "root",
    password = "<密碼>",
    database = "<已存在的資料庫名稱>" #若尚未創建可不加這段
)
cursor = testdb.cursor()
```

### 建立資料庫

```python=
cursor.execute("CREATE database <資料庫名稱>")
```

### 建立資料表

```python=
cursor.execute("CREATE table <Table 名稱> (id INTEGER AUTO_INCREMENT PRIMARY Key, name VARCHAR(20), img VARCHAR(200))")
```

### 插入資料

```python=
sqlstuff = "INSERT INTO pokeshiny (name, img) VALUES (%s, %s)"

records = [(Jack, Jack.png), (Mary, Mary.png)]

cursor.executemany(sqlstuff, records)
pokedb.commit()
```

###### tags: `實作` `Python` `MySQL`