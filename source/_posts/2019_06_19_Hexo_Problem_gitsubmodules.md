---
title: 問題 - Hexo 部署上 Github 後頁面全空了
tags:
  - 問題
  - Hexo Blog
  - Git
categories:
  - 問題
date: 2019-06-19
top_img: /img/problem.jpg
cover: /img/problem.jpg
---
# [問題] Hexo 部署上 Github 後頁面全空了

昨天晚上回到家想改個 Blog 的文章，於是就很正常的把在公司 `git push` 的程式碼 `git pull` 下來，沒想到改完 `hexo d -g` 部署上去後，頁面全空了

### 問題描述 :

通常我們使用別人的主題時都是 `git clone -b master <主題地址> themes/<主題名稱>` 這樣直接 clone 到自己的專案內

這時因為**別人的專案 ( themes ) 也有它自己的 `.git` 檔**，所以在我們的專案 `git push` 時，是不會提交 themes 上去的，導致我們在別台電腦 `git pull` 下來時，根本沒有拉到主題文件夾

![](https://i.imgur.com/exSD6jj.png)

* 在我們的專案 `git push` 後，Github 只顯示主題名稱文件夾，但沒有內容

為了要提交 themes 上去，我們要**在 themes 文件夾中 `git push` 一次，然後在我們的專案 `git push` 一次**

### 解決問題 :

**在這邊可以用 Git submodules 更好的解決此問題**

#### Git submodules

* Git submodules 可以允許你將一個 Git 倉庫做為另一個 Git 倉庫的子目錄

* 使用方法 : 

    1. `git submodule add <主題地址> themes/<主題名稱>` : 以 git submodule 取代 clone

        ![](https://i.imgur.com/y4jhMEt.png)

        * 以這樣提交後，Github 的主題文件夾就會連結到對方的專案去了

    2. `git submodule update --remote` : 若作者有更新主題，便可用這種方式更新
    3. `git clone --recursive` : 若要 clone 此包含 submodules 的倉庫，用此指令可以自動初始化每一個 submodules，並更新

### 參考 : [在 hexo 中使用 git submodules 管理主題](https://juejin.im/post/5c2e22fcf265da615d72c596?fbclid=IwAR2ci80WnaE6aXhJdji-4U9gleRc57PgBcAc28gJKrctLl7DXMJgIo_EUNQ)

###### tags: `問題` `Hexo Blog` `Git`