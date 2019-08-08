---
title: 筆記 - 機器學習 有效的改善模型
date: 2019-08-08 13:38:15
tags:
  - 筆記
  - 機器學習
  - 改善模型
categories:
  - 筆記
keywords:
description:
top_img: /img/note.jpg
cover: /img/note.jpg
---
# [筆記] 機器學習 有效的改善模型

### 改善方法

* 取得更多訓練資料
* 嘗試較少的特徵量
* 嘗試較多的特徵量
* 嘗試多項式的特徵量
* 嘗試增加 λ
* 嘗試減少 λ

### 機器學習診斷法 ( Machine learning diagnostic )

#### 評估假設 ( Evaluating a Hypothesis )

* 我們的假設可能使訓練樣本的誤差較小，但仍然不準確 ( 因為過度擬合 )，這時就必須評估假設
* 為了評估假設，將訓練樣本分為兩組 :

    1. 70% 的隨機數據當訓練集
    2. 30% 的隨機數據當測試集

* 過程 :

    1. 使用新的訓練集學習出 θ 和最小化的 J<sub>train</sub>(θ)
    2. 使用新的測試集計算出測試集錯誤率 ( The test set error ) J<sub>test</sub>(θ)

* 測試集錯誤率 ( The test set error )

1. 線性回歸 :

    ![](https://i.imgur.com/RESZL8X.png)

2. 邏輯回歸 :

    ![](https://i.imgur.com/sRZ5b3F.png)

    * 判斷是否錯誤

    ![](https://i.imgur.com/7P8RcuT.png)

    * 算出錯誤率

#### 模型選擇 ( Model selection )

* 鑑於具有需多不同的多項式模型，我們必須要有方法來做模型的選擇
* 將訓練樣本分為三組 :

    1. 60% 的隨機數據當訓練集
    2. 20% 的隨機數據當交叉驗證集 ( cross validation set )
    3. 20% 的隨機數據當測試集

* 過程 :

    1. 使用新的訓練集來學習出每個多項式的 θ 和最小化的 J<sub>train</sub>(θ)
    2. 使用新的交叉驗證集來計算出交叉驗證集錯誤率，找出錯誤率最小的模型 d
    3. 最後使用新的測試集算出泛化誤差 J<sub>test</sub>(θ(d))

* 此方法，測試集並沒有參與訓練

#### 分辨偏差與方差 ( Bias vs. Variance )

![](https://i.imgur.com/yIcEqii.png)

* 高偏差 ( High bias ) 屬於擬合不足 ( underfitting ) :

    * J<sub>train</sub>(θ)、J<sub>CV</sub>(θ) 都很高
    * J<sub>train</sub>(θ) ≈ J<sub>CV</sub>(θ)

* 高方差 ( High Variance ) 屬於過度擬合 ( overfitting ) :

    * J<sub>train</sub>(θ) 較低
    * J<sub>CV</sub>(θ) >> J<sub>train</sub>(θ)

#### 如何選擇 λ

![](https://i.imgur.com/tCVgnVa.png)

* 當 λ 過大時，會導致擬合不足，而當 λ 過小時，會導致過度擬合
* 如何選擇模型與 λ :

    1. 先列出一個 λ 列表 ( 例如 : λ ∈ {0, 0.01, 0.02, 0.04, 0.08, 0.16, 0.32, 0.64, 1.28, 2.56, 5.12, 10.24} )
    2. 建立一組擁有多項式的模型
    3. 使用每個 λ，各學習出 θ ( 使用正規化學習 )
    4. 使用交叉驗證集來算出錯誤率 J<sub>CV</sub>(θ) ( 不使用正規化，或使 λ = 0 )
    5. 選擇 J<sub>CV</sub>(θ) 最低的作為最佳組合
    6. 使用此最佳組合 θ、λ，查看在 J<sub>test</sub>(θ) 上是否運作良好

```Octave=
function [lambda_vec, error_train, error_val] = validationCurve(X, y, Xval, yval)

% λ 列表
lambda_vec = [0 0.001 0.003 0.01 0.03 0.1 0.3 1 3 10]';

error_train = zeros(length(lambda_vec), 1);
error_val = zeros(length(lambda_vec), 1);

for i = 1:length(lambda_vec)

  % 使用每個 λ，各學習出 θ
  lambda = lambda_vec(i);
  theta = trainLinearReg(X, y, lambda);
  
  % 計算出訓練集與交叉驗證集的錯誤率
  error_train(i) = linearRegCostFunction(X, y, theta, 0);
  error_val(i) = linearRegCostFunction(Xval, yval, theta, 0);

endfor

end
```

* 在 Octave 上的選擇 λ 函式

#### 學習曲線

* 在極少數資料 ( 1、2 或 3 ) 的訓練集，容易產生錯誤率為 0，因為很容易找到符合這些資料的多項式，因此 :

    * 隨訓練資料越多，函數的誤差增加
    * 在到達某個資料量後，誤差值將穩定下來

* 高偏差 ( High bias ) :

    * 資料量少 : J<sub>train</sub>(θ) 較低，J<sub>CV</sub>(θ) 較高
    * 資料量多 : J<sub>train</sub>(θ) ≈ J<sub>CV</sub>(θ)，都很高
    * 如果繼續增加訓練資料，並沒有太大改善

    ![](https://i.imgur.com/YKmkZIS.png)

* 高方差 ( High Variance ) :

    * 資料量少 : J<sub>train</sub>(θ) 較低，J<sub>CV</sub>(θ) 較高
    * 資料量多 : J<sub>train</sub>(θ) 隨資料量越大而增加， J<sub>CV</sub>(θ) 隨著減少且不平穩，J<sub>train</sub>(θ) < J<sub>CV</sub>(θ)，兩個差異仍然很大
    * 如果繼續增加訓練資料，有助於改善

    ![](https://i.imgur.com/0Ma9WVM.png)
    
```Octave=
function [error_train, error_val] = learningCurve(X, y, Xval, yval, lambda)

m = size(X, 1);

error_train = zeros(m, 1);
error_val = zeros(m, 1);


for i = 1:m

  X_train = X(1:i, :);
  y_train = y(1:i);
  
  % 訓練出 θ
  theta = trainLinearReg(X_train, y_train, lambda);
  
  % 計算出訓練集與測試集的錯誤率
  error_train(i) = linearRegCostFunction(X_train, y_train, theta, 0);
  error_val(i) = linearRegCostFunction(Xval, yval, theta, 0);

endfor

end
```

* 在 Octave 上的學習曲線函式

### 總結

* 解決高偏差 ( High bias ) :

    * 嘗試較多的特徵量
    * 嘗試多項式的特徵量

        ```Octave=
        function [X_poly] = polyFeatures(X, p)

        X_poly = zeros(numel(X), p);

        X_poly(:, 1) = X;

        for i = 2:p

          X_poly(:, i) = X .* X_poly(:, i - 1)

        endfor

        end
        ```

        * 在 Octave 上算出單特徵量多次方的函式

    * 嘗試減少 λ

* 解決高方差 ( High Variance ) :

    * 取得更多訓練資料
    * 嘗試較少的特徵量
    * 嘗試增加 λ

* 診斷神經網路

    * 較少參數的神經網路

        * 容易產生擬合不足 ( underfitting )
        * 計算成本較低

    * 較多參數的神經網路

        * 容易產生過度擬合 ( overfitting )
        * 計算成本較高
        * 可以使用正規化來解決過度擬合

    * 如何選擇 Hidden layer 數量

        * 使用多種 Hidden layer 數量不同的模型
        * 再用交叉驗證集選出效果較佳的

* 模型複雜度

    * 模型複雜度低

        * 高偏差、低方差
        * 對於訓練數據和測試數據都有較大的誤差

    * 模型複雜度高

        * 低偏差、高方差
        * 非常擬合訓練數據，但對測試數據有較大的誤差

###### tags: `筆記` `機器學習` `改善模型`