---
title: 問題 - 如何在 Hexo 上使用數學式
date: 2019-08-13 17:25:57
tags:
  - 問題
  - Hexo Blog
  - mathJax
categories:
  - 問題
top_img: /img/problem.jpg
cover: /img/problem.jpg
---
# [問題] 如何在 Hexo 上使用數學式

平常很習慣先用 HackMD 寫好筆記或文章後，再複製起來貼到 Hexo 上，但最近寫的筆記需要使用到數學式，一開始在 HackMD 撰寫渲染都很正常，但一貼到 Hexo 上，數學式就出錯了

![](https://i.imgur.com/iPKFyM4.png)

* 在 Hexo 上的顯示

![](https://i.imgur.com/7eFcUWF.png)

* 正常的顯示

### 問題描述 :

因為 Hexo 本身沒有支援數學式的顯示，所以必須更換渲染引擎使用 mathJax 來渲染數學式

### 解決問題 :

這邊是使用 `hexo-theme-melody` 的主題，所以是依照他們的文件解決

1. 更換渲染引擎

    ```shell=
    $ npm uninstall hexo-renderer-marked --save
    ```

    * 先將 Hexo 原本的渲染引擎 `marked` 刪除，在部落格的根目錄執行

    ```shell=
    $ npm install hexo-renderer-kramed --save
    ```

    * 在安裝上新的渲染引擎 `kramed`，在部落格的根目錄執行

2. 設定 `melody.yml.yml`

    ```yaml=
    mathjax:
      enable: true
      cdn: https://cdn.jsdelivr.net/npm/mathjax/MathJax.js?config=TeX-AMS-MML_HTMLorMML
    ```

    * 將此段程式碼加入或修改主題的設定檔 `melody.yml` 中，也許是其他名字

3. 設定 `_config.yml`

    ```yaml=
    #kramed
    kramed:
      gfm: true
      pedantic: false
      sanitize: false
      tables: true
      breaks: true
      smartLists: true
      smartypants: true
    ```

    * 將此段程式碼加入根目錄的 `_config.yml` 中

### 參考 : 

* [hexo-theme-melody mathjax](https://molunerfinn.com/hexo-theme-melody-doc/third-party-support.html#mathjax)

###### tags: `問題` `Hexo Blog` `mathJax`