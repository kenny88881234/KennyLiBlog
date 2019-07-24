---
title: 筆記 - 機器學習 邏輯回歸
date: 2019-07-19 09:38:22
tags:
  - 筆記
  - 機器學習
  - 邏輯回歸
categories:
  - 筆記
keywords:
description:
top_img: /img/note.jpg
cover: /img/note.jpg
---
# [筆記] 機器學習 邏輯回歸

用於分類 ( Classification )

### 邏輯回歸算法 ( Logistic Regression )

`hθ(x) = g(θᵀx)` : 邏輯回歸算式

#### 邏輯函數 ( Logistic Function )

* 又稱 S 函數( Sigmoid Function )

* ![](https://i.imgur.com/JoFwfc8.png)

* 此函數圖如下 :

![](https://i.imgur.com/5nhFFfr.png)

* 此函數可以將任何實數映射到 (0, 1) 區間內
* 假如 hΘ(x) = 0.7，那預測為 1 的機率是 70%，為 0 的機率是 30%

    * `hθ(x) = P(y=1 | x;θ) = 1 − P(y=0 | x;θ)`
    * `P(y=0 | x;θ) + P(y=1 | x;θ) = 1`

```Octave=
function g = sigmoid(z)

g = zeros(size(z));
g = 1 ./ (1 + e .^ -z);

end
```

* 在 Octave 上的邏輯函數函式

#### 決策邊界 ( Decision Boundary )

* 為了得到 0 與 1，按此轉換 :

    `hθ(x) ≥ 0.5 → y = 1`
    `hθ(x) < 0.5 → y = 0`

* 套入邏輯函數 g 中 :

    `g(z) ≥ 0.5 when z ≥ 0`

* 再套入邏輯回歸算式 :

    `hθ(x) = g(θᵀx) ≥ 0.5 when z = θᵀx ≥0`

* 結論 :

    `θᵀx ≥ 0 → y = 1`
    `θᵀx < 0 → y = 0`

* 此時 `θᵀx = 0` 為決策邊界

    假設 `θ = [5 -1 0]`
    ⇒ `5 + (-1)x₁ + 0x₂ ≥ 0 → y = 1`
    ⇒ `x₁ ≤ 5 → y = 1`
    ⇒ `x₁ = 5` 為決策邊界

#### 代價函數 ( Cost Function )

不能使用與線性回歸相同的代價函數，因為邏輯函數會導致代價函數，產生多個局部最小值，使它不會是個凸函數 ( convex function )

![](https://i.imgur.com/16FsaIJ.png)

⇒ `Cost(hθ(x), y) = -ylog(hθ(x)) - (1 - y)log(1 - hθ(x))`
⇒ ![](https://i.imgur.com/Yq4Ec4L.png)

`Cost(hθ(x), y) = 0 if hθ(x) = y`
`Cost(hθ(x), y) → ∞ if y = 0 and hθ(x) → 1`
`Cost(hθ(x), y) → ∞ if y = 1 and hθ(x) → 0`

* 假如我們的假設函數 `hθ(x)` 與正確答案 `y` 相同，則代價函數為 0 ，相反則會趨於無限大

#### 梯度下降 ( Gradient Descent )

![](https://i.imgur.com/05LyUzd.png)

* 與線性回歸的算法相同，一樣需要同步更新 `θ` 的所有值

```Octave=
function [J, grad] = costFunction(theta, X, y)

m = length(y);
J = 0;
grad = zeros(size(theta));

h = sigmoid(X * theta);

predictions = - y' * log(h);
con_predictions = (1 - y)' * log(1 - h);

J = (1 / m) * (predictions - con_predictions);

grad = (1 / m) * X' * (h - y);

end
```

* 在 Octave 上的代價函數 + 梯度下降 ( 應用於邏輯函數 ) 函式

#### 預測

通常訓練出 `θ` 後，帶入要預測的數值，輸出大於等於 0.5，預測為 1，反之為 0

* 與線性回歸的算法相同，一樣需要同步更新 `θ` 的所有值

```Octave=
function p = predict(theta, X)

m = size(X, 1);
p = zeros(m, 1);

p = sigmoid(X * theta) >= 0.5;

end
```

* 在 Octave 上的預測函式

#### 高級優化算法

* Conjugate gradient
* BFGS
* L-BFGS
* 以上三個的優缺點 :

    * 優點 :

        * 不需要 learning rate α
        * 比梯度下降還快

    * 缺點 :

        * 較為複雜

```Octave=
function [jVal, gradient] = costFunction(theta)
  jVal = [...code to compute J(theta)...];
  gradient = [...code to compute derivative of J(theta)...];
end
```

* 代價函數的函式程式碼

```Octave=
options = optimset('GradObj', 'on', 'MaxIter', 100);
initialTheta = zeros(2,1);
   [optTheta, functionVal, exitFlag] = fminunc(@costFunction, initialTheta, options);
```

* 使用 `optimset()` 與 `fminunc()` 來使用優化算法

    * 使用 `optimset()` 設定 `GradObj` 為 `on`，將設置梯度打開，設定 `MaxIter` 為 `100`，設定100次迭代
    * 給予 `fminunc()` 代價函數、θ 的初始向量、剛剛設定的 options

#### 多類別分類 : One-vs-all

`y ∈ {0,1...n}` : 有 n + 1 種類別

`hθ⁽¹⁾(x) = P(y = 0 | x;θ)`
`hθ⁽²⁾(x) = P(y = 0 | x;θ)`
. . .
`hθ⁽ⁿ⁾(x) = P(y = 0 | x;θ)`

* 需要使用到 n + 1 個邏輯回歸分類器 ( Logistic Regression Classifier )
* 每個分類器為分類**其中一個類別**與**其他類別**，例如 : 0 類別與不是 0 的類別、1 類別與不是 1 的類別...等等

![](https://i.imgur.com/bevfnR3.png)

* 預測新數值時，將新數值帶入每個分類器，取輸出值最大的分類器，也就是最大機率是此分類

###### tags: `筆記` `機器學習` `邏輯回歸`