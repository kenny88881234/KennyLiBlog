---
title: 筆記 - Octave 基礎
date: 2019-07-12 09:01:03
tags:
  - 筆記
  - 程式語言
  - 機器學習
  - Octave
categories:
  - 筆記
keywords:
description:
top_img: /img/note.jpg
cover: /img/note.jpg
---
# [筆記] Octave 基礎

### 運算語法

* `+` : 加
* `-` : 減
* `*` : 乘
* `/` : 除
* `^` : 次方

### 邏輯語法

* `==`
* `~=` : 不等於
* `&&`
* `||`
* `xor()`

### 矩陣創建

* `A = [1 2; 3 4; 5 6]` : 創建矩陣
* `v = n:i:m` : 生成 n 到 m，間隔 i 的矩陣
* `v = n:m` : 生成 n 到 m，間隔 1 的矩陣
* `ones(n, m)` : 生成都是 1 的矩陣，n rows，m columns
* `zeros(n, m)` : 生成都是 0 的矩陣，n rows，m columns
* `rand(n, m)` : 生成 0 到 1 內隨機數字的矩陣，n rows，m columns
* `randn(n, m)` : 生成常態分配的矩陣，n rows，m columns
* `hist(變數, n)` : 生成 n 個數據的直方圖
* `eye(n)` : 生成 n*n 的單位矩陣
* `magic(n)` : 生成 n*n 矩陣，內涵元素為 1 ~ n * n，每行每列斜對角總和相同

### 矩陣操作

#### 查看

* `disp(A)` : 將 A 矩陣打印出來
* `A(n, m)` : 查看 n row，m column 的值，`:` 代表全部
* `A([n m], :)` : 查看 n row 和 m row 所有的值
* `size(A, n)` : 查看 A 是幾乘幾的矩陣，n 可以不輸入，1 為查看 row，2 為查看 column
* `length(A)` : 查看 A 的最大維度
* `who` : 查看當前所有的矩陣
* `whos` : 詳細查看當前所有的矩陣
* `max(A)` : 查看 A 向量的最大值與索引位置，若 A 為矩陣查看 A 矩陣內每個 column 的最大值
* `max(A, [], n)` : 查看 A 矩陣內的最大值，n = 1 為查看 column，2 為查看 row
* `min(A)` : 查看 A 向量的最小值與索引位置，若 A 為矩陣查看 A 矩陣內每個 column 的最小值
* `min(A, [], n)` : 查看 A 矩陣內的最小值，n = 1 為查看 column，2 為查看 row
* `find(A < n)` : 查看元素小於 n 的位置

#### 修改

* `A = [A, [i; j; k]]` : 給 A 矩陣新增一行 column，值為 i、j、k
* `A(:)` : 將 A 矩陣的所有元素排成一行向量
* `V = [A B]` : 將 A、B 矩陣左右合併
* `V = [A; B]` : 將 A、B 矩陣上下合併
* `V = A(n:m)` : 將 A 矩陣 n ~ m 筆資料放進 V
* `clear A` : 刪除 A 矩陣，單個 clear 是刪除所有矩陣

#### 載入存入

* `load <檔名>.<副檔名>` : 載入檔案
* `save <檔名>.mat A` : 將 A 矩陣存入檔案
* `save <檔名>.txt A -ascii` : 將 A 矩陣存入檔案，存成 text

### 矩陣運算

* `A * B` : A、B 矩陣相乘
* `A .* B` : A、B 矩陣純量相乘
* `A .^ n` : A 矩陣的 n 次方
* `n ./ A` : n 常數與 A 矩陣純量相除
* `log(A)` : 對 A 矩陣取 log
* `exp(A)` : 對 A 矩陣取 exp
* `abs(A)` : 對 A 矩陣取絕對值
* `A'` : A 矩陣的轉置矩陣
* `A < 3` : true 回傳 1，false 回傳 0
* `sum(a)` : a 向量的總和
* `sum(A, n)` : A 矩陣的總和，1 為 column，2 為 row
* `prod(a)` : a 向量的總積
* `floor(A)` : A 矩陣取地板函數，捨去正小數至最近整數
* `ceil(A)` : A 矩陣取天花板函數，加入正小數至最近整數
* `pinv(A)` : A 矩陣的反矩陣

### 繪製

* `plot(n, m, 'r')` : 繪圖，n 為 x 軸，m 為 y 軸，'r' 為顏色，可以不加
* `hold on` : 讓已生成的圖像保持顯示，可以持續在上繪圖
* `xlable('<x 軸名稱>')` : 給 x 軸命名
* `ylable('<y 軸名稱>')` : 給 y 軸命名
* `legend('<第一個圖>', '<第二個圖>')` : 給每個繪圖命名
* `title('<標題>')` : 給此圖命名
* `print -dpng '<檔名>.png'` : 儲存此圖為圖檔
* `close` : 關掉顯示中的繪圖
* `figure(n); plot(n, m);` : 生成繪圖於編號 n 圖像，可以同時顯示多張圖像
* `subplot(n, m, i)` : 生成 n * m 格子的圖像，目前使用第 i 個格子
* `axis([n, m, i, j])` : 修改 x 軸 ( n ~ m )、y 軸 ( i ~ j ) 範圍
* `clf` : 清空圖像上的繪圖
* `imagesc(A)` : 將 A 矩陣可視化
* `colorbar` : 顯示代表數值的顏色表
* `colormap gray` : 將顏色表改為灰階

### 控制語法

#### for

```Octave=
for i = 1:10, %1 到 10
    <待執行程式碼>;
end;
```

#### while

```Octave=
i = 1;
while <條件>,
    <待執行程式碼>;
    i = i + 1;
end;
```

#### if

```Octave=
if <條件>,
    <待執行程式碼>;
elseif <條件>,
    <待執行程式碼>;
else
    <待執行程式碼>;
end;
```

### 函式

```Octave=
function y = squareThisNumber(x)

y = x^2
```

* 將此文件存為 `.m` 檔
* `addpath('<檔案位址>')` : 這樣 Octave 就能找到此函式

```Octave=
function [y1, y2] = squareThisNumber(x)

y1 = x^2
y2 = x^3
```

* 函式可返回多個值

### 其他

* `PS1('>> ');` : 可以簡化命令前字符
* 賦予值時加上 `;` : 可以不將賦予後的結果打印出來
* `help` : 幫助
* `pwd` : Octave 當前路徑
* 可用 `,` 連接多條命令
* `exit`、`quit` : 關閉 Octave

###### tags: `筆記` `程式語言` `機器學習` `Octave`