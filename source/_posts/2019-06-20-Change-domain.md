---
title: 實作 - 如何更改 Github-Page 的網域
date: 2019-06-20 10:58:21
tags:
  - 實作
  - Hexo Blog
  - Git
categories:
  - 實作
keywords:
description:
top_img: /img/code.jpg
cover: /img/code.jpg
---
# [實作] 如何更改 Github-Page 的網域

### 1. 首先要先有個自己的域名

* 用買的或使用免費的都可以
* 這裡推薦免費域名網站 : [NCTU.ME](https://nctu.me/)

    ![](https://i.imgur.com/Zj4kX0Y.png)

    * 這是交大免費提供的網域服務，交大學生有學校信箱就能直接使用，校外人士就必須申請一個TWID帳號進行實名認證

1. 申請帳號 : 交大信箱 or TWID帳號
2. 申請網域 : 網域管理 → 新增網域

    * 每個帳號只能申請三個域名
    * 域名格式為 : <輸入的字元>.nctu.me

    ![](https://i.imgur.com/fJT8fTq.png)

3. 新增DNS紀錄 : DNS管理 → 新增紀錄

    ![](https://i.imgur.com/Za4AXuQ.png)

    在 A 紀錄底下填上你要綁定的服務商的 DNS Server IP
    Github 的 DNS Server IP 是 185.199.108~111.153 其中一個
    在終端機 `ping <your name>.github.io` 就可以看到

    **也可以用 CNAME 紀錄，改成填上 `<your name>.github.io`**

    ![](https://i.imgur.com/wVvR8dk.png)

    等待一下，DNS 設定會在1、2分鐘內設定完成

### 2. 綁定網域

* 到 Github 專案的 Settings 頁面
* 在 GitHub Pages 下的 custom domain 欄位中填上剛剛申請的域名
* 按下 save 就完成了

![](https://i.imgur.com/7USxt9j.png)

**`hexo d -g` 會導致 Github 上的 `CNAME` 消失**

* 解決方法 : 在本地端目錄底下的 `source` 建一個 `CNAME` 並打上域名

```=
cd blog //你的hexo根目錄名
cd source
touch CNAME
```

###### tags: `實作` `Hexo Blog` `Git` `網域`