---
title: 筆記 - 機器學習 基礎與線性回歸
date: 2019-07-12 08:58:45
tags:
  - 筆記
  - 機器學習
  - 線性回歸
categories:
  - 筆記
keywords:
description:
top_img: /img/note.jpg
cover: /img/note.jpg
---
# [筆記] 機器學習 : 基礎與線性回歸

### 定義 :
 
>  A computer program is said to learn from experience E with respect to some class of tasks T and performance measure P, if its performance at tasks in T, as measured by P, improves with experience E.
  
![](https://i.imgur.com/pBIYQVb.png)

* 使用訓練資料產出一個函式 `h`，再用 `h` 去判斷 input `x` 產出 output `y`

### Octave for Microsoft Windows

* 主要用於數值分析的軟體，在初學機器學習很好用
* 下載地址 : [Octave 官方網站](http://wiki.octave.org/Octave_for_Microsoft_Windows)

![](https://i.imgur.com/DOyKeIN.png)

* 下載地址

![](https://i.imgur.com/Dsv02Tn.png)

* 下載 installer 後不斷 `next` 就能安裝完成了

P.S.下載任何版本都可以，但不要下載 `Octave 4.0.0`，此版本有重大的 bug

### Supervised learing ( 監督式學習 )

* 定義 : 機器去學習事先標記過的訓練範例 （ 輸入和預期輸出 ） 後，去預測這個函數對任何可能出現的輸入的輸出
* 函數的輸出可以分為兩類 :

    * 回歸分析 ( Regression ) : 輸出連續的值，例如 : 房價
    * 分類 ( Classification ) : 輸出一個分類的標籤，例如 : 有或沒有

### Unsupervised learing ( 非監督式學習 )

* 定義 : 沒有給定事先標記過的訓練範例，自動對輸入的資料進行分類或分群
* 常用於分群，有兩類應用 :

    * 聚類 ( Clustering ) : 將資料集中的樣本劃分為若干個通常是不相交的子集，例如 : 分類成不同類別的新聞
    * 非聚類 ( Non-clustering ) : 例如 : 雞尾酒會演算法，從帶有噪音的資料中找到有效資料，可用於語音辨識

### 線性回歸算法 ( Linear Regression )

* `hΘ(x) = Θ₀ + Θ₁x₁` : 線性回歸算式
* `m` : 資料量
* `x⁽ⁱ⁾` : i 代表第 i 筆資料

#### 代價函數 ( Cost Function )

![](https://i.imgur.com/2UNCKfj.png)

* 算出代價函數

```Octave=
function J = costFunctionJ(X, y, theta)

m = size(X,1);
predictions = X * theta;
sqrErrors = (predictions - y).^2;

J = 1 / (2 * m) * sum(sqrErrors);
```

* 在 Octave 上的代價函數函式

![](https://i.imgur.com/ryFygNy.png)

* 找出代價函數的最小值，來找出 Θ₀、Θ₁

#### 使用梯度下降 ( Gradient descent ) 將函數 J 最小化

1. 初始化 Θ₀、Θ₁ ( Θ₀=0, Θ₁=0 也可以是其他值)
2. 不斷改變 Θ₀、Θ₁ 直到找到最小值，或許是局部最小值

![](https://i.imgur.com/lDD0Y7J.png)

* 梯度下降公式，不斷運算直到收斂，Θ₀、Θ₁ 必須同時更新
* α 後的公式其實就是導數 ( 一點上的切線斜率 )
* α 是 learning rate

![](https://i.imgur.com/PLkKjd4.png)

* 正確的算法

![](https://i.imgur.com/CmwvFgc.png)

* 錯誤的算法，沒有同步更新

**Learning Rate α**

* α 是 learning rate，控制以多大幅度更新 Θ₀、Θ₁
* 決定 α 最好的方式是隨著絕對值的導數更新，絕對值的導數越大，α 越大
* α 可以從 0.001 開始 ( 每次 3 倍 )
* α 太小 : 收斂會很緩慢
* α 太大 : 可能造成代價函數無法下降，甚至無法收斂

#### 結合梯度下降與代價函數

![](https://i.imgur.com/kR0TFuh.png)

* 將代價函數帶入梯度下降公式
* 以**所有樣本**帶入梯度下降公式不斷尋找 Θ₀、Θ₁，在機器學習裡稱作批量梯度下降 ( batch gradient descent )

#### 多特徵線性回歸 ( Linear Regression with multiple variables)

* `hΘ(x) = Θ₀x₀ + Θ₁x₁ + Θ₂x₂ + ... + Θₙxₙ` : 多特徵線性回歸算式，x₀ = 1
* `n` : 特徵量

![](https://i.imgur.com/pOI4Lqt.png)

#### 使用梯度下降解多特徵線性回歸

![](https://i.imgur.com/4QvQlBf.png)

* 相較於一元線性回歸，只是多出最後的 xⱼ

![](https://i.imgur.com/ATD0b1b.png)

* 拆開後

#### 特徵縮放 ( Feature Scaling ) 與均值歸一化 ( Mean Normalization )

* 目的 : 加快梯度下降，因為特徵值範圍相差過大會導致梯度下降緩慢

![](https://i.imgur.com/HTHEYLJ.png)

* `sᵢ` : 特徵縮放，通常使用數值範圍
* `μᵢ` : 均值歸一化，通常使用數值的平均

#### 多項式回歸 ( Polynomial Regression )

* 我們可以結合多種有關的特徵，產生一個新的特徵，例如 : 房子長、寬結合成房子面積
* 假如線性的 ( 直線 ) 函式無法很好的符合數據，我們也可以使用二次、三次或平方根函式 ( 或其他任何的形式 )

#### 正規方程 ( Normal Equation )

X = 各特徵值
y = 各結果

* 算式 : `(XᵀX)⁻¹Xᵀy`
* Octave : `pinv(X'*X)*X'*y`

在 Octave 裡我們通常用 `pinv` 而不是 `inv`，因為使用 `pinv` 就算 `XᵀX` 為不可逆，還是會給予 Θ 的值

* `XᵀX` 不可逆的原因 :

    * 多餘且無關的特徵值
    * 特徵值過多 ( m<=n )，刪除一些或正規化

#### 梯度下降 vs 正規方程

* 梯度下降

    * 優點 : 

        * 特徵量大時，可以正常運作
        * O(kn²)

    * 缺點 : 

        * 需要選擇 α
        * 需要不斷迭代

* 正規方程

    * 優點 : 

        * 不需要選擇 α
        * 不用迭代

    * 缺點 : 

        * 需運算 (XᵀX)⁻¹，所以當特徵量大時，會耗費很多運算時間 ( n > 10000 )
        * O(n³)

###### tags: `筆記` `機器學習` `線性回歸`