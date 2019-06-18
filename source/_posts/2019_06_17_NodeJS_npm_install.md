---
title: 安裝 - NodeJS、npm 的安裝與配置
tags:
  - 安裝
  - NodeJS
  - npm
categories:
  - 安裝
date: 2019-06-17
---
# [安裝] NodeJS、npm 的安裝與配置

### **新版 NodeJS 裡面就包含了 npm**

#### 1. 下載

* 下載地址 : [NodeJS官網](https://nodejs.org/en/)

安裝在此目錄底下 `C:\Program Files\nodejs`

* `node -v` 與 `npm -v` 檢查是否安裝成功

    ![](https://i.imgur.com/51nUFxM.png)

#### 2. 配置 npm 的全局模塊的存放路徑以及cache的路徑

1. 建立 node_global、node_cache 兩個資料夾

    ![](https://i.imgur.com/ymB5riF.png)
    
2. 在終端機輸入
 
    * `npm config set prefix "C:\Program Files\nodejs\node_global"`
    * `npm config set cache "C:\Program Files\nodejs\node_cache"`

#### 3. 配置系統環境變量

1. 右鍵 本機，內容 → 進階系統設定 → 進階 → 環境變數 → 系統變數底下 新增 NODE_PATH
2. 新增路徑 `C:\Program Files\nodejs\node_global\node_modules` 上去
3. 更改使用者變數底下的 Path 

    * `C:\Program Files\nodejs\node_modules` 更改為 `C:\Program Files\nodejs\node_global`
    * 沒有就新增

4. 重新開機

###### tags: `安裝` `NodeJS` `npm`