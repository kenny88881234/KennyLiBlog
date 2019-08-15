---
title: 筆記 - 機器學習 支持向量機 ( Support Vector Machines )
date: 2019-08-15 15:13:16
tags:
  - 筆記
  - 機器學習
  - 支持向量機SVM
categories:
  - 筆記
keywords:
description:
top_img: /img/note.jpg
cover: /img/note.jpg
---
# [筆記] 機器學習 支持向量機 ( Support Vector Machines )

* 簡稱 SVM
* 屬於監督式學習算法
* 主要用於找到一個決策邊界 ( decision boundary ) 讓兩類的邊界 ( margins ) 最大化
* 大間距分類器 ( Large margin classifiers )

### SVM 的代價函數
 
* ![](https://i.imgur.com/VKJLCuF.png)

    * 將 `邏輯回歸的代價函數` * $\dfrac{m}{λ}$
    * 可以將 `C` 看成 $\dfrac{1}{λ}$，優化目標相同，SVM 的 `C` 與邏輯函數的 `λ` 只是透過不同方法來控制權重

* 這裡 h<sub>θ</sub>(x) 的定義 :

    * h<sub>θ</sub>(x) = 1，if θ<sup>T</sup>x ≥ 0
    * h<sub>θ</sub>(x) = 0，otherwise

* cost<sub>1</sub>(z) 與 cost<sub>0</sub>(z) 的圖 :

    ![](https://i.imgur.com/V8Cm2zU.png)

    * 支持向量機的條件更加嚴格 :

        * h<sub>θ</sub>(x) = 1，if θ<sup>T</sup>x ≥ 1
        * h<sub>θ</sub>(x) = 0，if θ<sup>T</sup>x ≤ -1

* `C` 值的大小 :

    * 當 `C` 非常大 :

        * 相當於 `λ` 非常小
        * 對於誤差點，也會進行很好的擬合
        * 造成過度擬合的情況

    * 當 `C` 非常小 :

        * 相當於 `λ` 非常大
        * 降低過度擬合的情況

### SVM 的推導

#### 內積

![](https://i.imgur.com/3seajNr.png)

* $||u||$ = 向量長度 = $\sqrt{u_1^2 + u_2^2}$
* p = v 向量投影在 u 向量的長度 ( 當 u、v 夾角大於 90 度時，p 就會是負的 )
* u<sup>T</sup>v = $p \cdot ||u||$ = u<sub>1</sub>v<sub>1</sub> + u<sub>2</sub>v<sub>2</sub>

#### SVM 如何選擇決策邊界

* ![](https://i.imgur.com/HbOSutr.png) = $\dfrac{1}{2}(θ_1^2 + θ_2^2)$ = $\dfrac{1}{2}(\sqrt{θ_1^2 + θ_2^2})^2$ = $\dfrac{1}{2}||θ||^2$
* θ<sup>T</sup>x<sup>(i)</sup> = $p^{(i)} \cdot ||θ||$ = $θ_1x_1^{(i)} + θ_2x_2^{(i)}$

    * θ<sup>T</sup>x<sup>(i)</sup> = $p^{(i)} \cdot ||θ||$ ≥ 1，if $y^{(i)}$ = 1
    * θ<sup>T</sup>x<sup>(i)</sup> = $p^{(i)} \cdot ||θ||$ ≤ -1，if $y^{(i)}$ = 0

首先要知道向量 θ 會與決策邊界垂直，且 θ<sub>0</sub> = 0 表示決策邊界通過原點，糟糕的決策邊界與良好的決策邊界如下 :

![](https://i.imgur.com/G5ldInk.png)

* 此為糟糕的決策邊界 ( 綠線 ) 與其向量 θ ( 藍線 )

    * 將每個 x<sup>(i)</sup> 投影在向量 θ 上，得到 p<sup>(i)</sup>
    * 當 p<sup>(i)</sup> 都較小時，為了滿足 $p^{(i)} \cdot ||θ||$ ≥ 1，$||θ||$ 就必須非常大
    * 但當 $||θ||$ 非常大時，就會使 $\dfrac{1}{2}||θ||^2$ 也就是代價函數跟著變大，所以 SVM 就不會選擇此決策邊界

![](https://i.imgur.com/9Dn6Ohw.png)

* 此為良好的決策邊界 ( 綠線 ) 與其向量 θ ( 藍線 )

    * 同上得到 p<sup>(i)</sup>
    * 當 p<sup>(i)</sup> 都較大時，$||θ||$ 就可以小一點
    * $||θ||$ 較小時，$\dfrac{1}{2}||θ||^2$ 也就是代價函數跟著變小，所以 SVM 就會選擇此決策邊界

### 核函數 ( Kernels )

* 用來打造非線性的支持向量機

#### 新的特徵值

* x 與標記點 ( l<sup>(1)</sup>、l<sup>(2)</sup>、l<sup>(3)</sup>、... ) 通過相似度函數計算出新的特徵值 f<sub>1</sub>、f<sub>2</sub>、f<sub>3</sub>、...

    * f<sub>1</sub> = Similarity(x, l<sup>(1)</sup>) = $exp(-\dfrac{||x - l^{(1)}||^2}{2σ^2})$
    * f<sub>2</sub> = Similarity(x, l<sup>(2)</sup>) = $exp(-\dfrac{||x - l^{(2)}||^2}{2σ^2})$
    * f<sub>3</sub> = Similarity(x, l<sup>(3)</sup>) = $exp(-\dfrac{||x - l^{(3)}||^2}{2σ^2})$

* 相似度函數是一種核函數，這裡用的是高斯核函數 ( Gaussian Kernel )

#### 核函數與相似度函數

* f<sub>1</sub> = Similarity(x, l<sup>(1)</sup>) = $exp(-\dfrac{||x - l^{(1)}||^2}{2σ^2})$

* 如果 x ≈ l<sup>(1)</sup> :

    * f<sub>1</sub> ≈ $exp(-\dfrac{0^2}{2σ^2})$ ≈ 1

* 如果 x 與 l<sup>(1)</sup> 相差很遠 :

    * f<sub>1</sub> ≈ $exp(-\dfrac{(Large Number)^2}{2σ^2})$ ≈ 0

* $σ^2$ 較小 :

    ![](https://i.imgur.com/pGAfUsU.png)

    * 當遠離 l<sup>(1)</sup> 時，f<sub>1</sub> 下降較快

* $σ^2$ 較大 :

    ![](https://i.imgur.com/OSC2kmI.png)

    * 當遠離 l<sup>(1)</sup> 時，f<sub>1</sub> 下降較慢

#### 應用示例

![](https://i.imgur.com/WWV1ZQh.png)

* 假設預測 y = 1，如果 θ<sub>0</sub> + θ<sub>1</sub>f<sub>1</sub> + θ<sub>2</sub>f<sub>2</sub> + θ<sub>3</sub>f<sub>3</sub> ≥ 0
* θ<sub>0</sub> = -0.5，θ<sub>1</sub> = 1，θ<sub>2</sub> = 1，θ<sub>3</sub> = 0

    * 新的樣本 x 與標記點 l<sup>(1)</sup> 相近

        * f<sub>1</sub> ≈ 1，f<sub>2</sub> ≈ 0，f<sub>3</sub> ≈ 0
        * θ<sub>0</sub> + θ<sub>1</sub> * 1 + θ<sub>2</sub> * 0 + θ<sub>3</sub> * 0 = -0.5 + 1 = 0.5 ≥ 0
        * 預測 y = 1

    * 新的樣本 x 與標記點 l<sup>(3)</sup> 相近

        * f<sub>1</sub> ≈ 0，f<sub>2</sub> ≈ 0，f<sub>3</sub> ≈ 1
        * θ<sub>0</sub> + θ<sub>1</sub> * 0 + θ<sub>2</sub> * 0 + θ<sub>3</sub> * 1 = -0.5 + 0 = -0.5 ≤ 0
        * 預測 y = 0

    * 新的樣本 x 與所有標記點都相遠

        * f<sub>1</sub> ≈ 0，f<sub>2</sub> ≈ 0，f<sub>3</sub> ≈ 0
        * θ<sub>0</sub> + θ<sub>1</sub> * 0 + θ<sub>2</sub> * 0 + θ<sub>3</sub> * 0 = -0.5 + 0 = -0.5 ≤ 0
        * 預測 y = 0

* 結論 :

    * 只要靠近標記點 l<sup>(1)</sup>、l<sup>(2)</sup> 就預測 y = 1，可以看出決策邊界如下 :

    ![](https://i.imgur.com/htWXXK1.png)

#### 如何選擇標記點

* 把每個訓練資料看作是一個標記點 ( landmark )，所以會有 m 個標記點

![](https://i.imgur.com/ylbcFRQ.png)

#### 支持向量機結合核函數

1. 使用 m 個資料 x<sup>(1)</sup> ~ x<sup>(m)</sup> 選擇出標記點 l<sup>(1)</sup> = x<sup>(1)</sup> ~ l<sup>(m)</sup> = x<sup>(m)</sup>
2. 算出新的特徵量 f<sub>1</sub><sup>(i)</sup> ~ f<sub>m</sub><sup>(i)</sup>，每個特徵量 x<sup>(i)</sup> 會算出 m 個 f<sup>(i)</sup>

    f<sub>1</sub><sup>(i)</sup> = Similarity(x<sup>(i)</sup>, l<sup>(1)</sup>)
    f<sub>2</sub><sup>(i)</sup> = Similarity(x<sup>(i)</sup>, l<sup>(2)</sup>)
    ...
    f<sub>m</sub><sup>(i)</sup> = Similarity(x<sup>(i)</sup>, l<sup>(m)</sup>)

3. 訓練出 θ

    ![](https://i.imgur.com/piBlgXP.png)

    * 上面的 n = m
    * ![](https://i.imgur.com/CrRNVdA.png) = θ<sup>T</sup>θ

        * 在實際運作上可能會是 θ<sup>T</sup> 乘上某個依賴核函數的矩陣再乘以 θ
        * 目的是使支持向量機能更有效率的運行

4. 預測

    * 如果 θ<sup>T</sup>f ≥ 0，預測 y = 1

P.S. 為什麼不在其他算法上使用核函數的概念

* 事實上是可以的，但會十分緩慢
* 因為支持向量機的設計細節上可以很適合核函數，但其他算法並沒有

#### 如何選擇支持向量機中的參數

* `C` ( = $\dfrac{1}{λ}$ ) :

    * 較大的 `C` : 相當於較小的 `λ`，低偏差 ( bias )，高方差 ( variance )，過擬合
    * 較小的 `C` : 相當於較大的 `λ`，高偏差 ( bias )，低方差 ( variance )，欠擬合

* $σ^2$

    * 較大的 $σ^2$ : 特徵量 f<sub>i</sub> 的變化較平滑，高偏差 ( bias )，低方差 ( variance )
    * 較小的 $σ^2$ : 特徵量 f<sub>i</sub> 的變化較不平滑，低偏差 ( bias )，高方差 ( variance )

```Octave=
function [C, sigma] = dataset3Params(X, y, Xval, yval)

C_trial = [0.01 0.03 0.1 0.3 1 3 10 30];
sigma_trial = C_trial;

m = size(C_trial, 2);

% error 最大就是 1
initial_error = 1;

for i = 1:m

  for j = 1:m

    model = svmTrain(X, y, C_trial(i), @(x1, x2) gaussianKernel(x1, x2, sigma_trial(j)));
    predictions = svmPredict(model, Xval);

    error = mean(double(predictions ~= yval));

    if error < initial_error

      initial_error = error;
      C_temp = C_trial(i);
      sigma_temp = sigma_trial(j);

    endif

  endfor

endfor

C = C_temp;
sigma = sigma_temp;

end
```

* 在 Octave 上選擇 `C` 與 $σ^2$ 的函式

### 使用支持向量機 ( SVM )

* 使用 SVM 的軟件包 ( EX : liblinear、libsvm、... ) 來解出 θ
* 使用支持向量機需要做的事 :

    * 選擇參數 `C`
    * 選擇核函數

        * 沒有使用核函數 ( 又稱線性核函數 )

            * 適合特徵量 ( n ) 大，訓練資料量 ( m ) 小
            * θ<sup>T</sup>x ≥ 0，預測 y = 1

        * 使用高斯核函數

            * 適合特徵量 ( n ) 小，訓練資料量 ( m ) 大
            * θ<sup>T</sup>f ≥ 0，預測 y = 1
            * 需要選擇參數 $σ^2$
            * 需要提供所使用的核函數

#### 高斯核函數 ( Gaussian Kernel )

![](https://i.imgur.com/tbBFJbM.png)

* f 為 f<sup>(i)</sup>，x1 為 x<sup>(i)</sup>，x2 為 l<sup>(j)</sup>

```Octave=
function sim = gaussianKernel(x1, x2, sigma)

x1 = x1(:);
x2 = x2(:);
sim = 0;

margin = sum((x1 - x2) .^ 2);
sigma2 = 2 * (sigma ^ 2);
sim = exp(- margin / sigma2);
    
end
```

* 在 Octave 上的高斯核函數函式

在使用高斯核函數前**應該先進行特徵縮放 ( Feature Scaling )**

* 這樣才能保證 SVM 會同等的關注到所有不同的特徵量

#### 其他的核函數

不管甚麼核函數都必須滿足默塞爾定理 (Mercer's Theorem)

* 多項式核函數 ( Polynomial Kernel )

    * k(x, l) = (x<sup>T</sup>l + constant)<sup>degree</sup>

* 字串核函數 ( String Kernel ) : 資料為字符串時有時會用到
* 卡方核函數 ( chi-square Kernel )
* 直方圖交叉核函數 ( histogram intersection Kernel )

#### 多類別分類

* 許多 SVM 的軟件包已經有內建多類別分類
* 如果沒有 :

    * 使用之前在邏輯回歸提過的 One-vs-all 方法
    * 訓練 K 個 SVM 去分類 K 個類別，得到 θ<sup>(1)</sup>、θ<sup>(2)</sup>、...、θ<sup>(K)</sup>
    * 將新數值帶入每個 SVM，取輸出值最大的 SVM ( (θ<sup>(i)</sup>)<sup>T</sup>x )

#### 邏輯回歸 vs SVMs

* `n` : 特徵量
* `m` : 訓練資料量

* 如果 n 很大 ( n ≥ m ) :

    * 使用邏輯回歸或沒有 kernel ( 線性核函數 ) 的 SVM

* 如果 n 很小，m 適中 ( n = 1 ~ 1000，m = 10 ~ 10000 ) :

    * 使用有高斯核函數的 SVM

* 如果 n 很小，m 很大 ( n = 1 ~ 1000，m = 50000+ ) :

    * 使用有高斯核函數的 SVM 會很緩慢
    * 增加特徵量後，使用邏輯回歸或沒有 kernel ( 線性核函數 ) 的 SVM

* 神經網路在這些條件下都能有很好的表現，但訓練速度相對緩慢

###### tags: `筆記` `機器學習` `支持向量機SVM`