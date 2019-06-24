---
title: 實作 - 讓 Google 能搜尋到自己的 Hexo Blog
date: 2019-06-24 16:59:45
tags:
  - 實作
  - Hexo Blog
  - Google Search Console
  - Sitemap
categories:
  - 實作
keywords:
description:
top_img: /img/code.jpg
cover: /img/code.jpg
---
# [實作] 讓 Google 能搜尋到自己的 Hexo Blog

### 1. 安裝 Hexo sitemap 套件

```=
$ npm install hexo-generator-sitemap --save
```

### 2. 加入 sitemap 路徑

打開 Blog 根目錄底下的 `_config.yml`

```=
#Sitemap
sitemap:
    path: sitemap.xml
```

### 3. 創建 sitemap 檔案

在終端機輸入 `$ hexo s`

* 在本地端地址後加上 `/sitemap.xml`
* EX : `http://localhost:4000/sitemap.xml`

![](https://i.imgur.com/iKyfGhk.png)

### 4. 向 Google 申請 Blog Search

進入 [Google Search Console](https://search.google.com/search-console/welcome)，請先登入

![](https://i.imgur.com/iJEe7Ch.png)

* 在這裡輸入 Blog 網址

![](https://i.imgur.com/nJUsmBH.png)

* 這裡有五種驗證方法，這邊選擇 Google 建議的 `HTML 檔案`

### 5. 將 HTML 檔案上傳到 Blog

1. 將 Google 提供的 HTML 檔案下載放到根目錄的 `source` 底下
2. 直接 `hexo d -g` 會出現這個 html 也被主題渲染的問題

    ![](https://i.imgur.com/yCFfyXK.png)

    * html 也被主題渲染了，會導致 Google 無法驗證

3. 在根目錄的 `_config.yml` 中找到 `skip_render` 參數，在後面加上 Google 提供的 html 檔名

    * `skip_render` 參數可以讓 Hexo 在建置靜態檔時忽略它，這樣這個 html 就不會被主題渲染了

    ![](https://i.imgur.com/8ci5ahv.png)

4. 按下驗證就成功囉

    ![](https://i.imgur.com/kuCORVE.png)

### 4. 提交 sitemap 網頁

* 在 Sitemap 新增 sitemap 網址 `<Blog 網址>/sitemap.xml` 就完成了

    ![](https://i.imgur.com/xvAvnKr.png)

###### tags: `實作` `Hexo Blog` `Google Search Console` `Sitemap`