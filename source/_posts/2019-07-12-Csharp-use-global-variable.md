---
title: 問題 - C# 如何使用全域變數
date: 2019-07-12 16:50:05
tags:
  - 問題
  - 程式語言
  - C#
  - 全域變數
categories:
  - 問題
keywords:
description:
top_img: /img/problem.jpg
cover: /img/problem.jpg
---
# [問題] C# 如何使用全域變數

在 C# 想宣告全域變數，在最外層直接宣告是不行的，C# 中並沒有宣告全域變數的方式，一般都是透過屬性 ( property ) 或靜態變數 ( static ) 來達成

### 靜態變數 ( static )

```csharp=
public class Global
{
    public static int x = 0;
}
```

### 屬性 ( property )

```csharp=
public class Global
{
    private string _x = "test";
    
    public string X
    {
        get //用來回傳給用戶讀取
        {
            return this._x;
        }
        set //開放給用戶修改
        {
            this._x = value;
        }
    }
    
    public string X { get; set;} //.Net 3.0 可以這樣寫
}
```

* 屬性的好處

    * 權限控制 : 在 set 前加上修飾詞 private 或 internal，代表此變數只有自己或內部能修改
    * 行為控制 : 可以在屬性判斷傳入的值是否符合規則、是否要觸發事件等等

###### tags: `問題` `程式語言` `C#` `全域變數`