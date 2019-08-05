---
title: 筆記 - 機器學習 神經網路 - 進階學習
date: 2019-08-05 11:16:48
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
# [筆記] 機器學習 神經網路 - 進階學習

### 代價函數與反向傳播算法 ( Backpropagation )

符號定義 :

* `L` : 神經網路中擁有的層數
* `sₗ` : 第 l 層中的單元個數
* `K` : 輸出的單元 ( 分類 ) 個數

#### 神經網路的代價函數

![](https://i.imgur.com/Qzk5RD8.png)

* 神經網路中的代價函數

    * 過往的正規化就是計算輸出層中的每個單元
    * 而在神經網路則是機算整個網路中的所有單元
    * 此 i ( 後者 ) 並非訓練資料的 i ( 前者 )

#### 反向傳播算法 ( Backpropagation )

* 是一種訓練神經網路的常見方法，與梯度下降結合使用，目的是找到一組能最大限度地減小誤差的權重

* 演算法 :

假設有訓練資料 `{(x⁽¹⁾, y⁽¹⁾) ... (x⁽ᵐ⁾, y⁽ᵐ⁾)}`

設定 `Δᵢⱼ⁽ˡ⁾ := 0 ( for all l, i, j )`

`For i = 1 to m : `

1. 設定 `a⁽¹⁾ := x⁽ⁱ⁾`
2. 計算 `a⁽ˡ⁾ ( for l = 2, 3, ..., L )`
3. 使用 `y⁽ⁱ⁾` 計算 `δ⁽ᴸ⁾ = a⁽ᴸ⁾ - y⁽ⁱ⁾`，L 代表 output layer
4. 使用 `δ⁽ˡ⁾ = ((Θ⁽ˡ⁾)ᵀδ⁽ˡ⁺¹⁾) .* a⁽ˡ⁾ .* (1 - a⁽ˡ⁾)` 計算 `δ⁽ᴸ⁻¹⁾, δ⁽ᴸ⁻²⁾, ..., δ⁽²⁾`

    * 激勵函數的導函數 : `g'(z⁽ˡ⁾) = a⁽ˡ⁾ .* (1 - a⁽ˡ⁾)`

5. `Δᵢⱼ⁽ˡ⁾ := Δᵢⱼ⁽ˡ⁾ + aⱼ⁽ˡ⁾δᵢ⁽ˡ⁺¹⁾`，向量化 : `Δ⁽ˡ⁾ := Δ⁽ˡ⁾ + δ⁽ˡ⁺¹⁾(a⁽ˡ⁾)ᵀ`

    * `Dᵢⱼ⁽ˡ⁾ := (1 / m)(Δᵢⱼ⁽ˡ⁾ + λΘᵢⱼ⁽ˡ⁾)`，if j ≠ 0
    * `Dᵢⱼ⁽ˡ⁾ := (1 / m) Δᵢⱼ⁽ˡ⁾`，if j = 0
    * ![](https://i.imgur.com/poAQcSC.png)

```Octave=
function [J grad] = nnCostFunction(nn_params, input_layer_size, hidden_layer_size, num_labels, X, y, lambda)

Theta1 = reshape(nn_params(1:hidden_layer_size * (input_layer_size + 1)), hidden_layer_size, (input_layer_size + 1));

Theta2 = reshape(nn_params((1 + (hidden_layer_size * (input_layer_size + 1))):end), num_labels, (hidden_layer_size + 1));

m = size(X, 1);
J = 0;
Theta1_grad = zeros(size(Theta1));
Theta2_grad = zeros(size(Theta2));

% 前向傳播算法

a1 = [ones(m, 1) X];

z2 = a1 * Theta1';
a2 = sigmoid(z2);
a2 = [ones(size(a2, 1), 1) a2];

z3 = a2 * Theta2';
a3 = sigmoid(z3);
h = a3;

yVec = zeros(m, num_labels);

for i = 1:m

  yVec(i, y(i)) = 1;

endfor

J = 1 / m * sum(sum(-1 * yVec .* log(h) - (1 - yVec) .* log(1 - h)));

regularator = (lambda / (2 * m)) * (sum(sum(Theta1(:, 2:end) .^ 2)) + sum(sum(Theta2(:, 2:end) .^ 2)));

J = J + regularator;

% 反向傳播算法

for t = 1:m
  
  % 設定 a1 為 X
  a1 = [1; X(t,:)'];
  
  % 計算 aL ( L = 1, 2, ..., L )
  z2 = Theta1 * a1;
  a2 = [1; sigmoid(z2)];
  
  z3 = Theta2 * a2;
  a3 = sigmoid(z3);
  
  % 計算 deltaL
  yy = ([1:num_labels] == y(t))';
  delta3 = a3 - yy;
  
  % 計算 delta ( L - 1, L - 2, ..., 2 )
  delta2 = (Theta2' * delta3) .* [1; sigmoidGradient(z2)];
  delta2 = delta2(2:end); % 不計算 bias unit
  
  % 計算 big delta
  Theta1_grad = Theta1_grad + delta2 * a1';
	Theta2_grad = Theta2_grad + delta3 * a2';
  
endfor

Theta1_grad = (1/m) * Theta1_grad + (lambda/m) * [zeros(size(Theta1, 1), 1) Theta1(:,2:end)];
Theta2_grad = (1/m) * Theta2_grad + (lambda/m) * [zeros(size(Theta2, 1), 1) Theta2(:,2:end)];

% 展開
grad = [Theta1_grad(:) ; Theta2_grad(:)];

end
```

* 在 Octave 上的前向傳播與反向傳播

```Octave=
function g = sigmoidGradient(z)

g = zeros(size(z));

g = sigmoid(z) .* (1 - sigmoid(z));

end
```

* 在 Octave 上的激勵函數的導函數

* `δⱼ⁽ˡ⁾` 就是 `aⱼ⁽ˡ⁾` 的偏差，也是代價函數的導數

    * ![](https://i.imgur.com/1XpEolE.png)

![](https://i.imgur.com/f82drs4.png)

* `δ₁⁽⁴⁾ = a₁⁽⁴⁾ - y⁽ⁱ⁾`
* `δ₂⁽³⁾ = Θ₁₂⁽³⁾ * δ₁⁽⁴⁾`
* `δ₂⁽²⁾ = Θ₁₂⁽²⁾ * δ₁⁽³⁾ + Θ₂₂⁽²⁾ * δ₂⁽³⁾`

### 實作反向傳播算法

#### 矩陣展開成向量與還原

* 在 `fminunc()` 的高級優化算法中，通常都需要傳入向量，這時我們就需要將矩陣展開成向量

```Octave=
thetaVector = [Theta1(:); Theta2(:); Theta3(:);]
deltaVector = [D1(:); D2(:); D3(:)]
```

* 假設有 Θ⁽¹⁾、Θ⁽²⁾、Θ⁽³⁾，D⁽¹⁾、D⁽²⁾、D⁽³⁾，展開成向量

```Octave=
Theta1 = reshape(thetaVector(1:110), 10, 11)
Theta2 = reshape(thetaVector(111:220), 10, 11)
Theta3 = reshape(thetaVector(221:231), 1, 11)
```

* 假設 Θ⁽¹⁾ 為 10 * 11 矩陣、Θ⁽²⁾ 為 10 * 11 矩陣、Θ⁽³⁾ 為 1 * 11 矩陣，將他們還原

##### 應用

```Octave=
創建初始化的 Θ⁽¹⁾、Θ⁽²⁾、Θ⁽³⁾
展開成向量 initialTheta
fminunc(@costFunction, initialTheta, options)
```

* 使用 `fminunc()`

```Octave=
function [jval, gradientVec] = costFunction(thetaVec)

    還原 thetaVec 成 Θ⁽¹⁾、Θ⁽²⁾、Θ⁽³⁾
    使用梯度下降或反向傳播算法算出 D⁽¹⁾、D⁽²⁾、D⁽³⁾ 和 J(Θ)
    展開 D⁽¹⁾、D⁽²⁾、D⁽³⁾ 成向量 gradientVec

end
```

* `costFunction()` 寫法

#### 梯度驗證 ( Gradient Checking )

* 通常使用反向傳播算法可能會出現許多 BUG，梯度驗證可以確保我們的反向傳播算法是如預期地工作的
* 算式 :

![](https://i.imgur.com/chqABww.png)

* 有多個 Θ 矩陣時 :

![](https://i.imgur.com/WXnqaQ1.png)

* 通常 `ϵ` 設定為 10⁻⁴，如果太小會導致數值問題

```Octave=
epsilon = 1e-4;

for i = 1:n,

  thetaPlus = theta;
  thetaPlus(i) += epsilon;
  
  thetaMinus = theta;
  thetaMinus(i) -= epsilon;
  
  gradApprox(i) = (J(thetaPlus) - J(thetaMinus))/(2*epsilon)
  
end;
```

* 在 Octave 上的梯度驗證
* 驗證 `gradApprox ≈ deltaVector`，確定反向傳播算法沒有錯誤後，就必須把梯度驗證關掉了，因為梯度驗證會導致程式執行非常的慢

#### 隨機初始化 ( Random Initialization )

* 在神經網路中，將 Θ 的所有權重初始化為 0 是不適合的
* 這樣會因為對稱而造成所有節點更新為相同的值
* 所以我們需要使用下面方法來隨機初始化 ( 打破對稱 Symmetry breaking ) Θ 的權重 :

![](https://i.imgur.com/teXtQj5.png)

* 將每個 `Θᵢⱼ⁽ˡ⁾` 初始化為 `[−ϵ, ϵ]` 之間的隨機值，此 `ϵ` 並非梯度驗證的 `ϵ`

```Octave=
%If the dimensions of Theta1 is 10x11, Theta2 is 10x11 and Theta3 is 1x11.

Theta1 = rand(10, 11) * (2 * INIT_EPSILON) - INIT_EPSILON;
Theta2 = rand(10, 11) * (2 * INIT_EPSILON) - INIT_EPSILON;
Theta3 = rand(1, 11) * (2 * INIT_EPSILON) - INIT_EPSILON;
```

* 在 Octave 上的隨機初始化概念

```Octave=
function W = randInitializeWeights(L_in, L_out)

W = zeros(L_out, 1 + L_in);

epsilon_init = 0.12;
W = rand(L_out, 1 + L_in) * 2 * epsilon_init - epsilon_init; 

end
```

* 在 Octave 上的隨機初始化

#### 總結

1. 神經網路的布局

    * input 單元數 = 特徵數 ( x )
    * output 單元數 = 類別數 ( y )
    * hidden 單元數通常越多越好，但計算的成本通常會隨之增加，必須取得平衡
    * 潛規則 : 

        * 至少有一層 hidden layer
        * 有兩層以上的 hidden layer，則建議每層單元數都相同

2. 訓練神經網路

    1. 隨機初始化權重 ( Θ )
    2. 實現向前傳播取得 hΘ(x⁽ⁱ⁾)
    3. 實現代價函數
    4. 實現反向傳播算法以計算導數 ![](https://i.imgur.com/poAQcSC.png)

        ```Octave=
        for i = 1:m,
            
            使用 (x⁽ⁱ⁾, y⁽ⁱ⁾) 執行向前傳播與反向傳播
            取得 a⁽ˡ⁾ 與 delta d⁽ˡ⁾ ( l = 2, ..., L )
            
        end;
        ```

    5. 使用梯度驗證確認反向傳播算法是否正確，**確認後關閉**
    6. 使用梯度下降或其他最優化功能計算代價函數最小值，並獲得 Θ 的權重

###### tags: `筆記` `機器學習` `神經網路`