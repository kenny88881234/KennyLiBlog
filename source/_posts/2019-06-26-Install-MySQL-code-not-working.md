---
title: 問題 - 安裝 MySQL 後環境變數被清空，導致終端機快速開啟 VS code 失效了
date: 2019-06-26 09:45:03
tags:
  - 問題
  - MySQL
  - VS code
categories:
  - 問題
keywords:
description:
top_img: /img/problem.jpg
cover: /img/problem.jpg
---
# [問題] 安裝 MySQL 後環境變數被清空了，導致終端機快速開啟 VS code 失效了

安裝完 MySQL 後，想上傳筆記到 Blog 上，就很高興地照著平常的習慣做，打開終端機 cd 進到 Blog 根目錄，然後 `code .` 快速開啟 VS code

![](https://i.imgur.com/1T68vVa.png)

咦，發生什麼事?

### 問題描述 :

進到環境變數中 `User 的使用者變數` 才發現，MySQL 把我的 `Path` 清空了，只留了他自己的 shell 位置...好樣的

![](https://i.imgur.com/bi6rfAP.png)

### 解決問題 :

這邊只要把環境變數加回去就解決了

1. 先找到 VS code `code` 這個檔案的位置
2. 一般來說會是在 `C:\Users\User\AppData\Local\Programs\Microsoft VS Code\bin`
3. 把這段路徑貼上變數值裡就可以了，間隔記得分號

    ![](https://i.imgur.com/TKA6x3v.png)
    
P.S.之前有增加過的環境變數都要記得加回去

###### tags: `問題` `MySQL` `VS code`