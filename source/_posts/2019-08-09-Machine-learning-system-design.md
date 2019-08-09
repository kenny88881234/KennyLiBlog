---
title: 筆記 - 機器學習 系統的設計
date: 2019-08-09 16:43:40
tags:
  - 筆記
  - 機器學習
  - 系統設計
categories:
  - 筆記
keywords:
description:
top_img: /img/note.jpg
cover: /img/note.jpg
---
# [筆記] 機器學習 系統的設計

### 提高機器準確性的方法

* 收集大量數據
* 開發複雜的特徵量
* 開發算法以不同方式處理輸入

P.S. 很難說哪個項目最有用

### 誤差分析 ( Error analysis )

1. 從簡單的算法開始，快速實作，並在交叉驗證集測試
2. 繪製出學習曲線，已確定算法該往哪個方向進行
3. 手動檢查交叉驗證集中錯誤的資料，查找這些資料大多數發生錯誤的特徵

    * 例如 : 分類垃圾郵件時，釣魚郵件大多數分類錯誤，就針對釣魚郵件下去改善算法

5. 將錯誤結果數值化很重要，否則很難評估算法性能

P.S. 分析時**最好用交叉驗證集測試**，如果用測試集去做誤差分析，算法只會越來越偏向測試集，在最後完成算法時，測試結果就會變得沒什麼參考價值

### 偏斜類 ( Skewed classes ) 問題

* 偏斜類是指大量的樣本傾向了某個類型，例如 :

    * 通過邏輯回歸來預測病人是否有罹患癌症

        * `y = 1` 為罹癌，`y = 0` 為沒有罹癌

    * 假設此錯誤率只有 1%，但我們的測試集中只有 0.5% 的人罹癌
    * 這時只要使用算法 :

        * h<sub>θ</sub>(x) = 0

    * 此算法的錯誤率甚至只有 0.5%

#### 查準率 ( Precision ) 與召回率 (Recall)

由上可知單純使用錯誤率並不能完善的評價模型好壞，所以我們需要兩種評價指標 :

* 查準率 ( Precision )
* 召回率 (Recall)

陽性 ( Positive ) :

* 預測值與實際值都為 1 時，稱為真陽性 ( True Positive )
* 預測值為 1，實際值為 0 時，稱為假陽性 ( False Positive )

陰性 ( Negative ) :

* 預測值與實際值都為 0 時，稱為真陰性 ( True Negative )
* 預測值為 0，實際值為 1 時，稱為假陰性 ( False Negative )

|          | 真正罹癌 1 | 真正沒罹癌 0 |
| -------- | -------- | -------- |
| 預測罹癌 1 | True Positive | False Positive |
| 預測沒罹癌 0 | False Negative | True Negative |

* 查準率 ( Precision )

    * $Precision = \dfrac{True Positive}{Predicated Positive} = \dfrac{True Positive}{True Positive + False Positive}$
    * 表示我們預測到的罹癌患者，有多少是真正有罹癌的
    * 查準率越高越好

* 召回率 ( Recall )

    * $Recall = \dfrac{True Positive}{Actual Positive} = \dfrac{True Positive}{True Positive + False Negative}$
    * 表示所有罹癌患者中，有多少罹癌患者被我們預測到了
    * 召回率越高越好

通過計算查準率與召回率，可以讓我知道分類模型到底好不好，就算測試集擁有偏斜類問題
P.S. 通常較稀少的類別會設定為 `y = 1`

#### 權衡查準率與召回率

* 假設我們想要有較高的把握才預測一個患者罹患癌症，避免非癌症患者接受不必要的治療，所以我們將臨界值提高

    * `y = 1` , h<sub>θ</sub>(x) ≥ 0.7
    * `y = 0` , otherwise
    * 照成高查準率，低召回率

* 假設我們想要一個不漏的預測罹癌患者，避免罹癌患者被忽略而延誤治療，所以我們將臨界值降低

    * `y = 1` , h<sub>θ</sub>(x) ≥ 0.3
    * `y = 0` , otherwise
    * 照成低查準率，高召回率

##### 使用 F<sub>1</sub> Score ( F score )比較查準率與召回率

* 可以使用 F<sub>1</sub> Score 來比較查準率與召回率在各臨界值上的表現，幫助選取較好的臨界值
* F<sub>1</sub> Score : $2 * \dfrac{Precision * Recall}{Precision + Recall}$
* 查準率與召回率都較高時，F<sub>1</sub> Score 才會較高

|          | Precision | Recall | F<sub>1</sub> Score |
| -------- | -------- | -------- | -------- |
| 算法 1 | 0.5 | 0.4 | 0.444 |
| 算法 2 | 0.7 | 0.1 | 0.175 |
| 算法 3 | 0.02 | 1.0 | 0.0392 |

* 可以看出 `算法 1` 是較合適的
* `算法 3` 雖然平均值比較高，但卻回到了偏斜類的問題，所有患者都預測為罹癌就會是 : 低查準率，高召回率

### 大數據之於機器學習

> It's not who has the best algorithm that wins. It's who has the most data.

* 所以大數據是很重要的，下圖是區分容易混淆單字的機器學習案例，可以發現數據越多時，各種算法表現都越來越優異 :

![](https://i.imgur.com/qENomHM.png)

數據量很大時，學習算法表現比較好的原理 : 

在使用多參數的算法時 ( 線性回歸 / 邏輯回歸擁有許多特徵量，神經網路擁有許多隱藏單元 )

* 夠多的特徵可以避免高偏差 ( 擬合不足 ) :

    * J<sub>train</sub>(θ) 將會降低

* 大數據可以避免多特徵容易引起的高方差 ( 過度擬合 ) :

    * J<sub>train</sub>(θ) ≈ J<sub>test</sub>(θ)

* 若將以上兩點整合 :

    * J<sub>test</sub>(θ) 將會降低

#### 大數據對機器學習有影響的情況

* 特徵值 x 包含足夠的信息使人類可以預測出 y
* 擁有很多參數的學習算法
* 若兩項都沒有，大數據對機器學習會沒什麼影響

###### tags: `筆記` `機器學習` `系統設計`