---
title: 筆記 - Python 基礎
date: 2019-06-23 22:28:27
tags:
  - 筆記
  - 程式語言
  - Python
categories:
  - 筆記
keywords:
description:
top_img: /img/note.jpg
cover: /img/note.jpg
---
# [筆記] Python 基礎

### 語言特性與熱門應用

* Python 是泛用的程式語言，強調易學好懂
* 最近常用在數據分析、人工智慧

### 安裝

下載地址 : [Python](https://www.python.org/)

![](https://i.imgur.com/wBDpXag.png)

* 勾起來，之後操作方便許多

### 資料與運算

#### 1. 變數與資料型態

* 數字 : `3.5`
* 字串 : `"hello"`
* 布林值 : `True`、`False` ( python 的 T 跟 F 為大寫 )
* 可變列表 ( List ) : `[3,4,5]`、`["hello","world"]`
* 固定列表 ( Tuple ) : `(3,4,5)`、`("hello","world")`
* 集合 : `{3,4,5}`、`{"hello","world"}`
* 字典 : `{"apple":"蘋果","data":"資料"}`
* 變數 : `data=3` data 為變數，不用宣告

#### 2. 運算

1. 數字

    * `7//6` : 整數除法
    * `2**3` : 2 的 3 次方

2. 字串

    * `"hell\"o` 輸出為 `hell"o` : 想在字串中加入雙引好可以用 `\` 跳脫
    * `"""hello"""` or `'''hello'''` : 在裡面的換行會直接呈現
    * 字串會對內部的字元編號 ( 索引 )，從 0 開始算起，假設 `s = "hello"`

        * `s[0]` 輸出為 `h`
        * `s[1:4]` 輸出為 `ell` : 輸出字串中編號 1 到 4 ( 不包含 )
        * `s[1:]` 輸出為 `ello`
        * `s[:4]` 輸出為 `hell`

3. 有序列表 ( List、Tuple )

    * List，假設 `grades = [12,60,15,70,90]`

        * `grades[1:4] = []` 輸出為 `[12,90]` : 連續刪除列表中編號 1 到 4 ( 不包含 )
        * `grades = grades + [12,33]` 輸出為 `[12,60,15,70,90,12,33]`

    * Tuple : 除了更動會出錯，其他都跟 List 一樣

3. 集合與字典

    * 集合，假設 `s1 = {3,4,5}` `s2 = {4,5,6,7}`

        * `10 in s1` 輸出為 `False`，`10 not in s1` 輸出為 `True`
        * `&` : 交集，`|` : 聯集，`-` : 差集，`^` : 反交集
        * `s = set("hello")` s 輸出為 `{'l','h','e','o'}` : 把字串拆解為集合
        
    * 字典，假設 `dic = {"apple":"蘋果","bug":"蟲蟲"}`

        * `test in dic` 輸出為 `False`，`test not in dic` 輸出為 `True`
        * `del dic["apple"]` : 刪除字典中的鍵值對
        * `dic = {x:x*2 for x in [3,4,5]}` dic 輸出為 `{3:6,4:8,5:10}` : 從列表資料產生字典

### 流程控制

* 在 python `{}` 用縮排 ( tab ) 取代

#### 1. 判斷式

```python=
if True:
    print("True 執行")
elif False:
    print("False 執行")
else:
    print("跑不到")
```

**python 目前不支援 switch**

#### 2. 迴圈

1. while

```python=
n = 1
while n<10:
    printf(n)
    n+=1
```

2. for

    * 迴圈跑列表

    ```python=
    for x in [3, 5, 1]:
        print(x)
    ```

    * 迴圈跑字串

    ```python=
    for x in "hello":
        print(x)
    ```

    * 迴圈跑range

    ```python=
    for x in range(5):
        print(x)
    ```

    `range(5)` : 0，1，2，3，4
    `range(3,7)` : 3，4，5，6

**迴圈最後可加 `else:`，else 在迴圈結束前執行，被 break 強制結束就不會執行 else**

### 函式、模組與封包

#### 函式 : 用來做程式的包裝，同樣邏輯可以重複利用

```python=
def add(n1 = 1,n2 = 2):
    result = n1 + n2
    return result

print(add(3, 4)) #7
print(add()) #3
print(add(n2 = 3,n1 = 4))
```

```python=
def avg(*ns): #無限不定參數資料
    sum = 0
    for n in ns:
        sum += n
    print(sum/len(ns))

avg(3, 4) #3.5
avg(3, 5, 10) #6.0

```

#### 模組 : 將程式寫在一個檔案中，此檔案可重複載入使用

1. 載入

```python=
import sys as system #載入模組並取別名 system
```

2. 使用

```python=
print(system.platform)
print(system.maxsize)
print(system.path) #印出模組的搜尋路徑
```

3. 內建模組與自訂模組

* 主程式

```python=
import sys #內建的模組
sys.path.append("modules") #在模組的搜尋路徑列表中新增路徑，這裡 "modules" 為資料夾
import geometry #自訂的模組，放在 "modules" 資料夾內
print(geometry.distance(1, 1, 5, 5))
```

* 模組 : 放在 `modules` 資料夾內

```python=
def distance(x1, y1, x2, y2):
    return ((x2 - x1)**2 + (y2 - y1)**2)**0.5
def slope(x1, y1, x2, y2):
    return (y2 - y1)/(x2 - x1)
```

#### 封包 : 用來整理、分類模組

* 檔案中的資料夾為封包，檔案為模組

1. 建立封包

    檔案配置 :

    * 專案資料夾

        * `主程式.py`
        * 封包資料夾

            * `__init__.py` : 必要
            * `模組一.py`
            * `模組二.py`

2. 使用封包

```python=
import 封包名稱.模組名稱 as 模組別名
```

### 類別與實體物件

* 封裝的變數或函式，統稱類別的屬性

#### 類別

1. 定義類別
```python=
class 類別名稱:
    定義封裝的變數
    定義封裝的函式
```

2. 使用類別

```python=
類別名稱.屬性名稱
```

#### 實體物件

1. 建立實體物件

```python=
class 類別名稱:
    def __init__(self): #定義初始化函式
        透過操作 self 來定義實體屬性
# 建立實體物件，放入變數 obj 中
obj = 類別名稱() #呼叫初始化函式
```

```python=
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

p = Point(1,5)
```

2. 使用實體物件

```python=
#實體物件.實體屬性名稱
p = Point(1,5)
print(p.x + p.y)
```

#### 實體方法

1. 建立實體方法

* 封裝在實體物件中的函式

```python=
class 類別名稱:
    def __init__(self): #定義初始化函式
        封裝在實體物件中的變數
    def 方法名稱(self,更多自訂參數):
        方法主體，透過 self 操作實體物件
# 建立實體物件，放入變數 obj 中
obj = 類別名稱() #呼叫初始化函式
```

```python=
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    def show(self):
        print(self.x, self.y)
p = Point(1,5)
```

2. 使用實體方法

```python=
#實體物件.實體方法名稱(參數資料)
p = Point(1,5) #建立實體物件
p.show() #呼叫實體方法
```

### 文字檔案的讀取與儲存

#### 1. 開啟檔案

```python=
檔案物件 = open(檔案路徑, mode = 開啟模式, encoding = 編碼)
```

* 開啟模式

    * `"r"` : 讀取模式
    * `"w"` : 寫入模式
    * `"r+"` : 讀寫模式

* 編碼

    * `"utf-8"` : 中文常用編碼

#### 2. 讀取檔案

* 讀取全部文字

```python=
檔案物件.read()
```

* 一次讀取一行

```python=
for 變數 in 檔案物件:
    從檔案依序讀取每行文字到變數中
```

* 讀取 JSON 格式

```python=
import json
讀取到的資料 = json.load(檔案物件)
```

#### 3. 儲存檔案

* 寫入文字

```python=
檔案物件.write("字串\n") #要換行就 \n
```

* 寫入 JSON 格式

```python=
import json
json.dump(要寫入的資料, 檔案物件)
```

#### 4. 關閉檔案

```python=
檔案物件.close()
```

#### 最佳實務

```python=
with open(檔案路徑, mode = 開啟模式) as 檔案物件:
    讀取或寫入檔案的程式
#以上區塊會自動、安全的關閉檔案，不需 close
```

### 亂數與統計模組

#### random

1. 載入模組

```python=
import random
```

2. 使用 random

```python=
random.choice([0, 1, 5, 8]) #從列表中隨機選取 1 個資料
random.sample([0, 1, 5, 8], n) #從列表中隨機選取 n 個資料
random.shuffle([0, 1, 5, 8]) #將列表的資料就地隨機調換順序
random.random() # 0 ~1 之間的隨機數亂數
random.uniform(n, m) # 取得 n ~ m 之間的隨機亂數
random.normalvariate(n, m) # 取得平均數 n，標準差 m 的常態分配亂數
```

常態分配範例 : 

![](https://i.imgur.com/Ilx8LtH.png)


#### statistics

1. 載入模組

```python=
import statistics
```

2. 使用 statistics

```python=
statistics.mean([1, 4, 6, 9]) #計算列表中數字的平均數
statistics.median([1, 4, 6, 9]) #計算列表中數字的中位數
statistics.stdev([1, 4, 6, 9]) #計算列表中數字的標準差
```

### 網路連線與公開資料串接

#### 網路連線

1. 載入模組

```python=
import urllib.request as request
```

2. 下載特定網址資料

```python=
with request.urlopen(網址) as response:
    data = response.read().decode("utf-8")
print(data)
```

#### 公開資料串接

1. 確認資料格式

    * JSON、CSV、或其他格式
    
2. 解讀格式

    * 解讀 JSON 使用內建的 JSON 模組

###### tags: `筆記` `程式語言` `Python`