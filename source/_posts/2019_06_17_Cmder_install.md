---
title: 安裝 - Cmder 的安裝與配置
tags:
  - 安裝
  - Cmder
categories:
  - 安裝
date: 2019-06-17
top_img: /img/install.jpg
cover: /img/install.jpg
---
# [安裝] Cmder 的安裝與配置

### **Cmder : 好用的window命令提示字元**

#### 1. 下載

* 下載地址 : [Cmder官網](https://cmder.net/)
* 分為Mini跟Full(Full包含了Git for Windows)

下載下來為壓縮檔，解壓縮後放在此目錄底下 `C:\Program Files\cmder1.3.11`

#### 2. 以管理員身分運行Cmder

右鍵 Cmder.exe，內容 → 相容性 → 勾選 以系統管理員的身分執行此程式

![](https://i.imgur.com/F1GxAzh.png)

#### 3. 以 win + R 開啟 Cmder

1. 新增路徑到系統的環境變數

    右鍵 本機，內容 → 進階系統設定 → 進階 → 環境變數 → 編輯 系統變數底下的 Path
    
    ![](https://i.imgur.com/wnpVR5O.png)

    新增路徑 `C:\Program Files\cmder1.3.11` 上去
    
    ![](https://i.imgur.com/uhkPMqa.png)

2. 啟動 win + R

    ![](https://i.imgur.com/UYIDMNK.png)

#### 4. 新增 Cmder 到右鍵選單

在管理員身分下執行`Cmder.exe /REGISTER ALL`
    
![](https://i.imgur.com/AtvE9k8.png)

#### 5. 將 λ 改成熟悉的 $

1. 打開 `C:\Program Files\cmder1.3.11\vendor\clink.lua`
2. 找到 `lambda = "λ"` 將 λ 改成 $
3. 重啟Cmder
4. 打開 `C:\Program Files\cmder1.3.11\vendor\profile.ps1`
5. 找到 `Microsoft.PowerShell.UtilityWrite-Host "nλ"` 將 λ 改成 $
6. 重啟Cmder

#### **切換C槽、D槽** : 在終端機輸入 `C:` or `D:`

###### tags: `安裝` `Cmder`