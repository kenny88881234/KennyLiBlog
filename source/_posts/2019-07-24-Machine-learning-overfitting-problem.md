---
title: 筆記 - 機器學習 過度擬合問題 ( overfitting )
date: 2019-07-24 09:17:11
tags:
  - 筆記
  - 機器學習
  - 過度擬合問題
categories:
  - 筆記
keywords:
description:
top_img: /img/note.jpg
cover: /img/note.jpg
---
# [筆記] 機器學習 過度擬合問題 ( overfitting )

### 過度擬合 ( overfitting ) 與擬合不足 ( underfitting )

![](https://i.imgur.com/L1q43Qf.png)


* 過度擬合 ( 右圖 )

    * 特徵量過多，資料量過少

    * 解決方法 : 

    1. 降低特徵量

        * 人工檢查，保留較重要的特徵量
        * 也有演算法 ( model selection algorithm ) 可以選擇何為較重要的特徵值

    2. 正規化

        * 可以保留所有的特徵量，但減少 θⱼ 的大小
        * 當擁有需多有用的特徵量時，正規化是很有效的使所有特徵量都會有貢獻

* 擬合不足 ( 左圖 )

    * 特徵量過少

### 正規化 ( Regularization )

#### 代價函數

原理 : 將 θⱼ 乘上較大的係數，可以使 θⱼ 大幅下降，降低一些特徵值的影響力

* 例如 : 

    ![](https://i.imgur.com/2svrYiq.png)

    * `θ₃`、`θ₄` 會大幅下降，甚至可以忽略
    * `θ₀ + θ₁x + θ₂x² + θ₃x³ + θ₄x⁴` → `θ₀ + θ₁x + θ₂x²`

![](https://i.imgur.com/O9dcw7Q.png)

* 正規化後的代價函數
* λ 為正規化參數，如果過大會因為 θⱼ ( j ≠ 0 ) 全都趨近於 0 導致 underfitting

#### 正規化線性回歸 ( Regularized Linear Regression )

![](https://i.imgur.com/rjcOHt5.png)

* 先將 `θ₀` 分離出來，因為正規化不須懲罰 `θ₀`
* 再加上正規化算式

![](https://i.imgur.com/jVGW2Y9.png)

* 將 `θⱼ` 提出來
* 此為正規化線性回歸


#### 正規化正規方程 ( Regularized Normal Equation )

![](https://i.imgur.com/ENR87dQ.png)

* 因為正規化不須懲罰 `θ₀`，所以最左上角為 0
* `λ⋅L` 可以使原本不可逆的 `XᵀX` 變成可逆的 `XᵀX + λ⋅L`

#### 正規化邏輯回歸 ( Regularized Logistic Regression )

![](https://i.imgur.com/YbflGko.png)

* 邏輯回歸的代價函數正規化
* 梯度下降的方法跟正規化線性回歸大同小異

```Octave=
function [J, grad] = costFunctionReg(theta, X, y, lambda)

m = length(y);
J = 0;
grad = zeros(size(theta));

h = sigmoid(X * theta);
shift_theta = theta(2:size(theta));
theta_reg = [0;shift_theta];

predictions = - y' * log(h);
con_predictions = (1 - y)' * log(1 - h);

J = (1 / m) * (predictions - con_predictions) + (lambda / (2 * m)) * theta_reg' * theta_reg;

grad = (1 / m) * (X' * (h - y) + (lambda * theta_reg));

end
```

* 在 Octave 上的正規化邏輯回歸函式

###### tags: `筆記` `機器學習` `過度擬合問題`