---
title: 筆記 - 機器學習 神經網路 ( Neural Network ) - 表層結構
date: 2019-07-26 10:57:34
tags:
  - 筆記
  - 機器學習
  - 神經網路
categories:
  - 筆記
keywords:
description:
top_img: /img/note.jpg
cover: /img/note.jpg
---
# [筆記] 機器學習 神經網路 ( Neural Network ) - 表層結構

### 為什麼使用神經網路

* 在特徵量多的情況下，在非線性回歸算法裡，會產生很多的特徵項，這並不是一個解決複雜非線性問題的好辦法
* 假設我們要分類 50 * 50 像素的灰階圖片 :

    * 此特徵量 n = 2500 ( RBG 的話就是 n = 7500 )
    * 那麼 `xᵢ * xⱼ` 的所有條件就有 2500 * 2500 / 2 ≈ 300 萬
    * 這樣的計算成本太高了

* 而神經網路是個能輕鬆解決非線性問題的算法
* 神經網路最初產生的目的是模擬人類的大腦，早期不盛行的原因是，當時硬體無法支撐如此龐大的運算

### 神經網路模型的運作

#### 神經網路的模型

![](https://i.imgur.com/wHWmm3q.png)

* 在前面輸入特徵 x₀ ~ xₙ
* x₀ 是一個偏置單元 ( bias unit )，始終為 1
* 最後的輸出則是我們假設函數的結果
* 在神經網路中最基本我們使用跟分類中相同的邏輯函數 ![](https://i.imgur.com/JoFwfc8.png)，有時會稱為激勵函數 ( sigmoid activation function )，之後也會有其他種的函數，例如 : TanH、ReLU
* theta 有時也被稱為權重 ( weights )

![](https://i.imgur.com/rlCPRlW.png)

* Layer 1 為 input layer
* Layer 2 為 hidden layer，在 input layer 和 output layer 之間的都是
* Layer 3 為 output layer

`aᵢ⁽ʲ⁾` 為在 Layer j 中的 activation unit i
`Θ⁽ʲ⁾` 為控制 Layer j 到 Layer j + 1 的權重矩陣

![](https://i.imgur.com/73pU4MO.png)

* `Θ⁽ʲ⁾` 的維度 ( dimension ) 為 sⱼ₊₁ * (sⱼ + 1)
* sⱼ 為在 Layer j 中的 unit 個數

#### 神經網路的計算過程

![](https://i.imgur.com/rlCPRlW.png)

* 假設 `x = a⁽¹⁾`
* `a⁽ʲ⁾ = g(z⁽ʲ⁾)`，例如 : `a⁽²⁾ = g(z⁽²⁾)`
* `z⁽ʲ⁾ = Θ⁽ʲ⁻¹⁾a⁽ʲ⁻¹⁾`，例如 : `z⁽²⁾ = Θ⁽¹⁾a⁽¹⁾`
* `hΘ(x) = a⁽ʲ⁾ = g(z⁽ʲ⁾)`，此 Layer j 為 output layer

### 神經網路的應用

#### 運算 OR

![](https://i.imgur.com/8iJbAvz.png)

* `hΘ(x) = g(-10 + 20x₁ + 20x₂)`


1. x₁ = 0 and x₂ = 0 then g(−10) ≈ 0
2. x₁ = 0 and x₂ = 1 then g(10) ≈ 1
3. x₁ = 1 and x₂ = 0 then g(10) ≈ 1
4. x₁ = 1 and x₂ = 1 then g(30) ≈ 1


* g() 為邏輯函數 ( sigmoid function )

#### 進階運算 XNOR

* AND : `Θ⁽¹⁾ = [-30 20 20]`
* NOR : `Θ⁽¹⁾ = [10 -20 -20]`
* OR : `Θ⁽¹⁾ = [-10 20 20]`

將三個結合後 :

![](https://i.imgur.com/kFjZ6lo.png)

* ![](https://i.imgur.com/gCpv7Tz.png)

* ![](https://i.imgur.com/NLvFxyo.png)

* ![](https://i.imgur.com/fW2CqLL.png)

1. `a⁽²⁾ = g(Θ⁽¹⁾x)`
2. `a⁽³⁾ = g(Θ⁽²⁾a⁽²⁾)`
3. `hΘ(x) = a⁽³⁾`

#### 多類別分類運算

* 在多類別分類時，並不是 `y = 1`、`y = 2`、`y = 3`、`y = 4`...這樣分類
* 而是有多個輸出來個別表示是否為此分類

![](https://i.imgur.com/sg3BjP6.png)

* 假設這次有 4 種分類，那輸出的所有種類如上

![](https://i.imgur.com/J9N4xIY.png)

* 神經網路的模型可以這樣設置

```Octave=
function [all_theta] = oneVsAll(X, y, num_labels, lambda)

m = size(X, 1);
n = size(X, 2);

all_theta = zeros(num_labels, n + 1);

X = [ones(m, 1) X]; %加入 x₀

initial_theta = zeros(n + 1, 1);

options = optimset('GradObj', 'on', 'MaxIter', 50);

for c = 1:num_labels

  [theta] = fmincg (@(t)(lrCostFunction(t, X, (y == c), lambda)), initial_theta, options);
  all_theta(c, :) = theta';

end

end
```

* 在 Octave 上的多類別分類器函式

```Octave=
function p = predictOneVsAll(all_theta, X)

m = size(X, 1);
num_labels = size(all_theta, 1);
p = zeros(size(X, 1), 1);

X = [ones(m, 1) X];  

predict = sigmoid(X * all_theta');
[predict_max, index_max] = max(predict, [], 2);
p = index_max;

end
```

* 在 Octave 上的預測計算函式

```Octave=
function p = predict(Theta1, Theta2, X)

m = size(X, 1);
num_labels = size(Theta2, 1);
p = zeros(size(X, 1), 1);

X = [ones(m, 1) X];

z2 = X * Theta1';
a2 = sigmoid(z2);

a2 = [ones(size(a2, 1), 1) a2]; %加入 a₀⁽²⁾

z3 = a2 * Theta2'
a3 = sigmoid(z3)

[a3_max, index_max] = max(a3, [], 2);
p = index_max;

end
```

* 在 Octave 上的預測計算函式 ( 多層神經網路 )

###### tags: `筆記` `機器學習` `神經網路`