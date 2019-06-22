---
title: 工具 - Git and SourceTree
tags:
  - 工具
  - Git
  - SourceTree
categories:
  - 工具
date: 2019-06-18
top_img: /img/tool.jpg
cover: /img/tool.jpg
---
# [工具] Git and SourceTree

## Git

### **Git : 分散式版本控制軟體**

一個 Git 目錄裡，可以分成三種區域 :

* 目前工作目錄 Working tree
* 暫存準備遞交區 Staging Area
* 儲存庫 Repository

![](https://i.imgur.com/oFsMceh.png)

而 Working tree 裡的檔案有四種狀態 :

* 沒有被追蹤的檔案 Untracked files
* 有修改、還沒準備要被遞交 Changes not staged for commit
* 有修改、準備要被遞交的檔案 ( 在 Staging Area ) Changes to be committed
* 已經被遞交的檔案 Committed

![](https://i.imgur.com/ainhgdG.png)

#### 1. SSH Git hub

* 終端機 :

1. 在終端機輸入 `ssh-keygen` 來產生金鑰
2. 進到金鑰存放的位置，通常是 `C:\Users\User\.ssh`
3. `vim id_rsa.pub` 使用文字編輯器打開後 `y` 兩次複製

* Git hub :

4. Git hub 右上角進到 Settings
5. SSH and GPG → keys New SSH key

![](https://i.imgur.com/StG9vfy.png)

6. Title 輸入想要得名稱，Key 貼上剛剛複製下來的密鑰

![](https://i.imgur.com/lKt0Lgs.png)

#### 2. 基本語法

1. `git init` : 初始化目錄
2. `git add .` : add 所有更動過的項目
3. `git commit -m "message"`
4. `git remote` : 可以檢視你已經設定好的遠端版本庫
5. `git push` : 推上 Github
6. `git pull` : 從 Github 將專案拉下來
7. `git branch` : 查看分支
8. `git checkout <分支名稱>` : 切換分支

**[命令大全](https://mitblog.pixnet.net/blog/post/44380918-%5Bgit%5D-git-%E6%8C%87%E4%BB%A4%E5%A4%A7%E5%85%A8%E3%80%81git-%E5%91%BD%E4%BB%A4%E5%A4%A7%E5%85%A8%E3%80%81basic-git-comman)**

##### 其他 :

`git push -f` : 強制推上 Github，小心使用
`git remote set-url origin git@github.com:<your name>/<your repository name>.git` : 如果有更改 Repository 名稱，可以使用這個修改要推上的 url
`git remote -v` : 查看目前連結的 Repository url

---

## SourceTree

### **SourceTree : 版本控管的工具**

#### 1. 下載

* 下載地址 : [SourceTree官網](https://www.sourcetreeapp.com/)

#### 2. 開啟

* 終端機輸入 `stree .` 可開啟

###### tags: `工具` `Git` `SourceTree`