---
title: 筆記 - Javascript 基礎
tags:
  - 筆記
  - 程式語言
  - Javascript
categories:
  - 筆記
date: 2019-06-17
top_img: /KennyLiBlog/img/note.jpg
cover: /KennyLiBlog/img/note.jpg
---
# [筆記] Javascript 基礎

### 語言特性

1. 動態型別語言

    * 單一變數的型別可能會隨時改變

2. 弱型別語言

    * 執行期間才檢查型別
    * 優點 : 開發快
    * 缺點 : 大專案難以維護，無法知道型別

        * typescript強制javascript宣告型別

3. 物件導向語言

    * 除了原始型別，其他都是物件

        * 原始型別 : Numbers、String、Booleans...等
        * 物件可以包含物件
        * value可以關聯function

        ![](https://i.imgur.com/X2tukto.png)

### javascript物件特性

1. javascript物件是個容器(Container)

    * 可以包含屬性(Property)和方法(Method、Function)

2. javascript物件其實是HashMap

    * 屬性名稱一定是字串
    * 取得屬性方法有兩種

        * 物件取法 : `car.name`
        * 陣列取法 : `car['name']`
    * 數字不能當變數

3. javascript沒有class，不需要建構式(constructor)就能建立物件

    * javascript的constructor就是函式，稱建構式函式
    * 使用new來透過建構式函式建立物件

    ```javascript=
    function person(first, last, age, eyecolor)
    {
        this.firstName = first;
        this.lastName = last;
        this.age = age;
        this.eyeColor = eyecolor;
    }
    var myFather = new person("John", "Doe", 50, "blue");
    ```
    * javascript沒有class，需借助prototype的特性
        
        * class用於定義這個物件有甚麼方法
    
4. javascript物件可以不須先行宣告就可自由擴充屬性

    ```javascript=
    var obj = {'a':1, 'b':2};
    //擴增屬性
    obj.c = 3;
    //刪除屬性
    delete obj.b;
    ```
    
5. 原生物件與宿主物件

    1. 原生物件(Native) : 例如Array、Data...等
    2. 宿主物件(Host) : 例如window物件和所有DOM物件

    * 如何分辨

        * 看物件能不能在瀏覽器底下執行

            * 只能在瀏覽器執行就是Host
            * 可以在非瀏覽器執行就是Native

### 物件、變數與型別關係

1. 物件

2. 變數

    * Number

        * javascript沒有浮點數，都是Number型別
        * NaN = "not a number"

    * String

        * 宣告 : `abc` "abc" 'abc'
        * 雙引號之內要雙引號必須加斜線 "\"" 這會是 "

    * Booleans

        * NaN == NaN 會是 true
        * 正確判斷要用三個等於

            ```javascript=
            console.log(1 == "1")
            //true
            console.log(1 === "1")
            //false
            ```
3. 型別

### 函式物件(function)

1. 函式是javascript的一級物件(first-class object)

    * 可被動態建立
    * 可以指定給變數
    * 有如同物件的特性

2. 函式表示法

    * 表示式

        * 具名表示

        ```javascript=
        var add = function add(a, b){return a+b;};
        ```

        * 匿名表示

        ```javascript=
        var add = function (a, b){return a+b;};
        ```

    * 宣告式

        * 具名宣告

        ```javascript=
        function add(a, b){return a+b;};
        ```

        * 匿名宣告

        ```javascript=
        function (a, b){return a+b;};
        ``` 

3. prompt

```javascript=
a = prompt("Enter passcode");
console.log(a)
```
![](https://i.imgur.com/vrXhDfX.png)

* [MDN.Math](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Math)

### Handling Events

1. Event handlers

    ```javascript=
    window.addEventListener("click", () => {
        console.log("You knocked?");
    });
    ```

2. Events and DOM nodes

    * HTNL是個樹，裡面的 < header >、< body >...等等就是DOM nodes

    ```javascript=
    let button = document.querySelector("button");
    button.addEventListener("click", () => {
        console.log("Button clicked.");
    });
    ```

    * function命名化

    ```javascript=
    let button = document.querySelector("button");
    function once() {
        console.log("Done.");
        button.removeEventListener("click", once);
    }
    button.addEventListener("click", once);
    ```

3. Event objects

    ```javascript=
    let button = document.querySelector("button");
    button.addEventListener("mousedown", event => {
        if (event.button == 0) {
            console.log("Left button");
        } else if (event.button == 1) {
            console.log("Middle button");
        } else if (event.button == 2) {
            console.log("Right button");
        }
    });
    ```

    * event可以自行命名
    * 舊寫法 : event 換成 function(event)

4. Propagation

    ```javascript=
    let para = document.querySelector("p");
    let button = document.querySelector("button");
    para.addEventListener("mousedown", () => {
        console.log("Handler for paragraph.");
    });
    button.addEventListener("mousedown", event => {
        console.log("Handler for button.");
        if (event.button == 2) event.stopPropagation();
    });
    ```

    * stopPropagation()可以阻止事件冒泡(event bubbling)
    * 事件冒泡(event bubbling) : 事件會往外傳遞

5. Default actions

    ```javascript=
    <a href="https://developer.mozilla.org/">MDN</a>
    <script>
        let link = document.querySelector("a");
        link.addEventListener("click", event => {
            console.log("Nope.");
            event.preventDefault();
        });
    </script>
    ```

    * preventDefault()可以阻止初始的行為

6. Key events

    ```javascript=
    window.addEventListener("keydown", event => {
        if (event.key == "v") {
          document.body.style.background = "violet";
        }
    });
    window.addEventListener("keyup", event => {
        if (event.key == "v") {
          document.body.style.background = "";
        }
    });
    ```
    
7. Scroll events

    * "scroll" : 滾輪滾動

8. Focus events

    * "focus" : ![](https://i.imgur.com/gldrD1L.png)

9. Load event

    * 讀取用

### 其他

1. const物件是可以改變的

    * ```const a = 1;```
        * a不能改變

    * ```const b = {};```
        * b可以改變

2. 宣告 var、let、const 的差異性

    * var : 函式作用域

        * fuction外才讀不到值

    * let、const : 區塊作用域

        * 區塊語句(if、else、for、while...等等)外就會讀不到值

###### tags: `程式語言` `Javascript`