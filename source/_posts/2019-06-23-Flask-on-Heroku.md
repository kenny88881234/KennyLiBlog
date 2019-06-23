---
title: 實作 - Flask 網站開發並部署上 Heroku
date: 2019-06-23 22:34:07
tags:
  - 實作
  - Python
  - Web 框架
  - Flask
  - Heroku
categories:
  - 實作
keywords:
description:
top_img: /img/code.jpg
cover: /img/code.jpg
---
# [實作] Flask 網站開發並部署上 Heroku

* Flask 是一種 python 中常用的 web 框架，python中常用的框架有 :

    * django
    * Flask
    * tornado

## 環境建置

### 基本流程

1. 安裝 Flask 套件
2. 建立專案資料夾，撰寫程式
3. 啟動伺服器，測試網站運作

### 安裝 Flask 套件

```=
$ pip install Flask
```

### 建立專案

1. 建立網站專案

* 隨便找個你想放的位置建一個資料夾

2. 撰寫程式

```python=
from flask import Flack
app = Flask(__name__) # __name__ 為 python 內建的變數，他會儲存目前程式在哪個模組下執行

@app.route("/") #函式的裝飾 ( Decorator )，以底下函式為基礎，提供附加的功能，這邊 "/" 代表根目錄
def home():
    return "Hello Flask"
    
if __name__ == "__main__": #如果以主程式運行
    app.run() #啟動伺服器
```

### 啟動伺服器

* 直接運行主程式，底下會有網址，連過去就看的到

## 部署上 Heroku 雲端伺服器

### 基本流程

1. 建立 Flask 專案描述檔
2. 安裝 Git
3. 到 Heroku 註冊帳號、建立應用
4. 安裝 Heroku 命令列工具
5. 將程式部署到 Heroku App

### 建立描述檔

* `runtime.txt` : 描述使用的 python 環境

    ```=
    python-<版本號>
    ```

* `requirements.txt` : 描述程式運作所需要的套件

    ```=
    Flask
    gunicorn #在 Heroku 上用
    ```

* `Procfile` : 告訴 Heroku 如何執行程式

    ```=
    web gunicorn <main檔名 ( app )>:<使用 Flask 的變數名稱 ( app )>
    ```

### 安裝 Git

* 下載地址 : [Git 官網](https://git-scm.com/)

### 註冊帳號

1. [Heroku 官網](https://www.heroku.com/) 註冊帳號
2. 建立應用程式 : 登入後找到 `Create new app`

### 安裝 Heroku 命令列工具

* 下載地址 : [Heroku CLI 下載地址](https://devcenter.heroku.com/articles/heroku-cli)

### 部署專案

1. 登入 Heroku : `$ herohu login`
2. 初始化專案 :

    ```=
    $ cd <本地端的專案名稱>
    $ git init
    $ heroku git:remote -a <Heroku 上的專案名稱>
    ```
3. 更新或部署專案 :

    ```=
    $ git add .
    $ git commit -m "message"
    $ git push heroku master
    ```
    
###### tags: `實作` `Python` `Web 框架` `Flask` `Heroku`