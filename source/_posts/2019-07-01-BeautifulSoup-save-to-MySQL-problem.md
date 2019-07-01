---
title: 問題 - BeautifulSoup 解析後無法存入 MySQL
date: 2019-07-01 15:22:56
tags:
  - 問題
  - Python
  - MySQL
  - BeautifulSoup
categories:
  - 問題
keywords:
description:
top_img: /img/problem.jpg
cover: /img/problem.jpg
---
# [問題] BeautifulSoup 解析後無法存入 MySQL

在練習爬蟲，使用 BeautifulSoup 分析 HTML 後，準備將資料存進 MySQL 時，出現 `Python 'navigablestring' cannot be converted to a MySQL type` 的錯誤

![](https://i.imgur.com/FwOoTQV.png)

### 問題描述 :

`navigablestring` 是 BeautifulSoup 中的一種類型 `<class 'bs4.element.NavigableString'>`，不是 MySQL 所認識的類型，所以會導致存入失敗

### 解決問題 :

只要將要存入的參數轉換類型就可以了，有兩種方法 :

* `str(<要轉換的參數>)`
* `<要轉換的參數>.encode('utf-8')`

P.S. 當然你的資料庫要能接受 UTF-8 編碼

```python=
testdb = MySQL.connect(
    host = "localhost",
    user = "root",
    password = "<密碼>",
    database = "<已存在的資料庫名稱>", #若尚未創建可不加這段
    charset='utf8'
)
cursor = testdb.cursor()
```

* 加上 `charset='utf8'` 可以保證連接的資料庫能接受 UTF-8 編碼

```python=
cursor.execute("CREATE database <資料庫名稱> DEFAULT CHARSET = utf8 COLLATE = utf8_general_ci")
```

* 在創建資料庫時，這樣就能接受 UTF-8 編碼

###### tags: `問題` `Python` `MySQL` `BeautifulSoup`