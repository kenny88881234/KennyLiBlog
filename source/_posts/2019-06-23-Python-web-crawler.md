---
title: 實作 - Python 實作 網路爬蟲 ( web crawler )
date: 2019-06-23 22:31:25
tags:
  - 實作
  - Python
  - 網路爬蟲
categories:
  - 實作
keywords:
description:
top_img: /img/code.jpg
cover: /img/code.jpg
---
# [實作] Python 實作 網路爬蟲 ( web crawler )

### 基本流程

1. 連線到特定網址，抓取資料
2. 解析資料，取得實際想要的部分

### 抓取資料

* 盡可能地，**讓程式模仿一個普通使用者的樣子**，因為許多網站不希望人家用程式去抓取他們的資料
* 必須包含 Headers

```python=
import urllib.request as req
#建立一個 Request 物件，附加 Request Headers 的資訊
url = "網址"
request = req.Request(url, headers = {
    "User-Agent":"需要的資訊" #到網頁 → F12 → Network → 通常是最上面的那個 → Headers Request → Headers → user-agent
})
```

### 解析資料

* JSON 格式

    * 使用內建的 JSON 模組來解析

* HTML 格式

    * 使用第三方套件 BeautifulSoup 來解析
    * 安裝 BeautifulSoup

        * 使用 pip 套件管理工具 ( 安裝python 時，就一起裝了 )

        ```=
        $ pip install beautifulsoup4
        ```

```python=
import bs4
root = bs4.BeautifulSoup(data, "html.parser") #解析html
print(root.title.string) #抓 title 標籤底下的 string

titles = root.find("div", class_ = "title") #尋找 class = "title" 的 div 標籤
print(titles.a.string)

titles = root.find_all("div", class_ = "title") #尋找所有 class = "title" 的 div 標籤
for title in titles:
    if title.a != None:
        print(title.a.string)
```

###### tags: `實作` `Python` `網路爬蟲`