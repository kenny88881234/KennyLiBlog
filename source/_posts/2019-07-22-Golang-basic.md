---
title: Golang-basic
date: 2019-07-22 16:17:37
tags:
  - 筆記
  - 程式語言
  - Golang
categories:
  - 筆記
keywords:
description:
top_img: /img/note.jpg
cover: /img/note.jpg
---
# [筆記] Golang 基礎

### 基本特性

* 泛用的程式語言
* 強調效能
* 易學
* 垃圾回收 ( 內存管理 )
* 編譯語言 ( Compiled language )

### 安裝

下載地址 : [Golang 官方網址](https://golang.org/dl/)

### 流程

1. 撰寫程式

```go=
package main //可執行的城市必須使用 main

import "fmt" //載入內建 fmt 封包，用來做基本輸出輸入

func main() {
	fmt.Println("Hello")
}
```

2. 建置 ( build ) : `go build <檔名>.go`

    ![](https://i.imgur.com/PPNK5AY.png)

    * 建置完會出現 `.exe` 檔

3. 執行程式 : 直接在終端機打上 `./` + `.exe` 檔的檔名

    ![](https://i.imgur.com/rNTc4GS.png)

### Workspaces

* src : 放置原始碼檔案
* pkg : 放置使用到的函式庫
* bin : 放置執行檔

### 資料、資料型態、變數

#### 資料型態

* int : 整數 ( int8、int16、int32、int64、uint8、uint16... )
* float64 : 浮點數 ( float32、complex128 )

    ```go=
    var x complex128 = complex(1, 2) //1 + 2i
    ```

* string : `""`，字串

    * String package

        ```go=
        Compare(a, b) //比較 a 與 b 字串
        Contains(str, substr) //判斷 str 是否包含 substr
        HasPrefix(str, prefix) //判斷 prefix 是否為 str 的前置字元
        Index(str, substr) //判斷 substr 在 str 第一個出現的位置
        Replace(str, old, new, n) //將 str 前 n 個 old 替換成 new
        TrimSpace(str) //去掉 str 頭尾的空白
        ToUpper(str) //轉成大寫
        ToLower(str) //轉成小寫
        ...
        ```

    * Strconv package

        ```go=
        Atoi(str) //將字串轉換為數字
        Itoa(num) //將數字轉換為字串
        FormatFloat(f, fmt, prec, bitSize) //將浮點數轉換為字串
        ParseFloat(str, bitSize) //將字串轉換為浮點數
        ...
        ```

* bool : 布林值
* rune : `''`，字元，實際存放整數，所以印出來會是整數

    * Unicode package

        ```go=
        IsDigit(r rune) //判斷 r 是否為十進制數字
        IsSpace(r rune) //判斷 r 是否為空白
        IsLetter(r rune) //判斷 r 是否為字母
        IsLower(r rune) //判斷 r 是否為小寫
        IsPunct(r rune) //判斷 r 是否為符號
        ToUpper(r rune) //轉成大寫
        ToLower(r rune) //轉成小寫
        ...
        ```

#### 資料型態轉換

`type(變數)` : 以資料型態把變數括起來，例如 :

```go=
int32(x)
```

#### 變數宣告

```go=
var <變數名稱> <資料型態名稱> //宣告變數
var <變數名稱> = 100 //宣告變數並賦予值，自動判別型別
<變數名稱> := 100 //宣告變數並賦予值，自動判別型別
```

* const

    ```go=
    const <變數名稱> = 100 //const 宣告為固定值，不可改變
    const ( //多個 const 宣告
        <變數名稱> = "Hi"
        <變數名稱> = 1000
    )
    ```

* iota

    ```go=
    const (
        <變數名稱> = iota //0
        <變數名稱> //1
    )
    ```

    iota 初始為 0，每次 const 都會讓 iota 初始化

#### 資料型態重命名

```go=
type IDnum int //重命名後，int 還是可以用
var pid IDnum
```

* 重命名後就可以使用新的資料型態名稱來宣告變數

#### 變數可視範圍 ( Variable Scope )

GO 裡面是以 `{}` 來分隔區塊的

* Universe block : 整個 GO 的程式碼
* Package block : package 內的程式碼
* File block : 檔案內的程式碼
* if、for、switch... : 語法內的程式碼
* switch、select 內的子句 : 子句內的程式碼

### 基本輸入與輸出

#### 載入內建封包

```go=
import "fmt"
```

#### 從終端介面輸入

```go=
fmt.Scanln(&變數名稱, &變數名稱, ...) //以換行為結束
fmt.Scan(&變數名稱, &變數名稱, ...) //以換行或空白為結束
fmt.Scanf("變數格式，EX : %d", &變數名稱) //跟 C 語言的用法一樣
```

* `&變數名稱` : 取得變數的指標 ( Pointer )

#### 印出到終端介面

```go=
fmt.Println(資料, 資料, ...) //換行
fmt.Print(資料, 資料, ...) //不換行
fmt.Printf("變數格式，EX : %d", 變數名稱) //跟 C 語言的用法一樣
```

### 指標 ( Pointer )

* `&` : 傳回變數或函式的地址
* `*` : 傳回此地址的值

```go=
var x int = 1
var y int
var ip *int //ip is pointer to int

ip = &x //ip now points to x
y = *ip //y is now 1
```

* `new()` : 這個函式用來創建變數，並給予變數指標

```go=
ptr := new(int)
*ptr = 3
```

### 結構控制

#### if

```go=
if <條件> { //條件沒有小括弧
    <待執行程式>
}
```

#### for loops ( Go 沒有 while，for 是唯一的循環語句 )

```go=
for <初始值>;<結束值>;<更新值> {
    <待執行程式>
}
```

```go=
<初始值>
for <結束值> { //類似 while
    <待執行程式>
    <更新值>
}
```

#### switch

```go=
switch <變數> { //Go 不需要 break
    case <變數為此執行此行>:
        <待執行程式>
    case <變數為此執行此行>:
        <待執行程式>
    default:
        <待執行程式>
}
```

```go=
switch { //switch 不用變數 tag 時，類似 if、else if
    case <條件>:
        <待執行程式>
    case <條件>:
        <待執行程式>
    default:
        <待執行程式>
}
```

### 複合型別

#### Arrays

* 固定空間的元素集合
* 以 `[]` 宣告與表示
* 索引從 0 開始
* 初始值為 0 或空字串

```go=
var x [5]int
x[0] = 2
fmt.Printf(x[1]) //印出為 0
```

```go=
var x [5]int = [5]{1, 2, 3, 4, 5}
```

* 宣告並給予初始值

```go=
x := [...]int {1, 2, 3, 4}
```

* `...` 可以用於不定數

```go=
x := [3]int {1, 2, 3}

for i, v range x {
fmt.Printf("ind %d, val %d", i, v)
}
```

* 在 for loop 使用 `range`，`range` 會回傳兩個數值，索引值與此索引內的值

#### Slices

* 可動態的增加空間，不需像 array 一樣指定大小
* 由三個元件組成

    * Pointer : 指向一個 array 的指標
    * Length : 資料實際長度，`len()` 可傳回 Slice 的長度
    * Capacity : 容量，`cap()` 可傳回 Slice 的容量

```go=
arr := [...]string {"a", "b", "c", "d", "e", "f", "g"}
s1 := arr[1:3] //{"b", "c"}
s2 := arr[2:5] //{"c", "d", "e"}
```

* s1、s2 為 Slices

```go=
sli := []int {1, 2, 3}
```

* 宣告方法，也可以不給予初始值

```go=
sli1 = make([]int, 10) //len = cap = 10
sli2 = make([]int, 10, 15) //len = 10,cap = 15
```

* 使用 `make()` 宣告

```go=
sli = make([]int, 0, 3)
sli = append(sli, 100) //len 會跟著增加
```

* 使用 `append()` 增加 Slice 內的元素

#### Hash Tables

* Key 和 value
* 優點 :

    * 較快的搜尋速度 : 比起 list 一個一個找快很多
    * 有意義的 Key : Key 非整數，是可以命名的字串，在撰寫程式時較容易辨別

* 缺點 :

    * 可能會有碰撞 : 兩個 Key hash 到同個位置，碰撞會使程式慢上一些，但發生機率極低

![](https://i.imgur.com/95GqlGl.png)

#### Maps

```go=
var idMap1 map[string]int //宣告
idMap2 = make(map[string]int) //以 make() 宣告
idMap3 := map[string]int { //宣告並給予值
    "joe" : 123
}
```

* 宣告

```go=
fmt.Println(idMap["joe"])
```

* 使用

```go=
idMap["jane"] = 456
```

* 新增

```go=
delete(idMap, "joe")
```

* 刪除

```go=
id, p := idMap["joe"]
fmt.Println(len(idMap))
```

* id 為 value，p 為是否存在此 Key，存在為 true
* `len()` 可查看此 Map 有多少鍵值對

```go=
for key, val := range idMap {
    fmt.Println(key, val)
}
```

* Map 在 for loop 的應用

#### Structs

* 多種資料型態的集合

```go=
type struct Person {
    name string
    addr string
    phone string
}
var p1 Person
```

* 宣告

```go=
p1 := new(Person)
```

* 以 `new()` 宣告並初始化

```go=
p1 := Person(name : "joe", addr : "a st.", phone : "123")
```

* 宣告並賦予值

```go=
p1.name = "joe"
x = p1.addr
```

* 賦予值與使用

### 協定與格式

#### RFC ( Requests for Comments )

```go=
import "net/http"

http.Get(www.uci.edu)
```

* 使用 HTTP 協定，GET、POST、PUT...等等

```go=
import "net"

net.Dial("tcp", "uci.edu:80")
```

* 使用於 TCP/IP 連線與撰寫 socket 程式

#### JSON ( JavaScript Object Notation )

* 是一種屬性鍵值對，類似 struct 或 map
* 鍵值可以是任意基本資料型態，Bool、Number、String、Array...等等
* unicode 編碼
* RFC 之一
* 可以結合多種資料型態

```go=
p1 := Person(name : "joe", addr : "a st.", phone : "123")
```

* struct

```json=
{
    "name":"joe",
    "addr" : "a st.",
    "phone" : "123"
}
```

* JSON，相當於上面的 struct

```go=
import "encoding/json"

barr, err := json.Marshal(p1)
```

* 將 struct `p1` 轉換成 JSON 格式

```go=
var p2 Person

err := json.Unmarshal(barr, &p2)
```

* 將 JSON 格式轉換成 go object 放進 `p2`
* 會自動適應 Person 的各項屬性，若無會回傳 err

#### File

**ioutil**

```go=
import "ioutil"

data, e := ioutil.ReadFile("test.txt")
```

* ReadFile 可以讀取檔案內容，不須開啟與關閉檔案，如果檔案過大可能會造成錯誤

```go=
data = "Hello, world"

err := ioutil.WriteFile("outfile.txt", data, 0777)
```

* WriteFile 會創建一個檔案並寫入資料後關閉，`0777` 為權限

**os**

```go=
import "os"

f, err := os.Open("dt.txt") //開啟檔案

barr := make([]byte, 10)
nb, err := f.Read(barr) //讀取檔案前 10 byte 進 barr，nb 為讀取多少 byte

f.Close() //關閉檔案
```

* 開啟檔案後，讀取並關閉

```go=
f, err := os.Create("outfile.txt")

barr := []byte{1, 2, 3}
nb, err := f.Write(barr)
nb, err := f.WriteString("Hi") //寫入字串
```

* 創建檔案並寫入

###### tags: `筆記` `程式語言` `Golang`