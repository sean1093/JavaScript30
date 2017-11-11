# 02 - JS and CSS Clock

## 目標

* 利用 JavaScript & CSS 製作出動態的時鐘

* 目標學習利用 CSS 控制移動與 JavaScript 的簡易時間操作

* DEMO: http://sean1093.github.io/JavaScript30/02%20-%20JS%20and%20CSS%20Clock/index-SEAN.html


## 實作

在一開始預設就已經有了基本的 CSS class，我們只需要做一些調整即可

我們的三根指針除了都擁有共用的 hand class 之外，各自分別有著自己的 class，這些可以拿來作為 querySelector 的選擇目標，或是拿來分別實作不一樣的指針長相都可以。

由於我們 hand 的設定，一開始三根指針會是呈現水平並且相疊在一起。

```css
.hand {
    width:50%;
    height:6px;
    background:black;
    position: absolute;
    top:50%;
}
```

接著下一步我們就要讓他跟著時間來跳動
我們利用標準的 Data 物件來抓取現在的時分秒

```js
const now = new Date();
const seconds = now.getSeconds();
const mins = now.getMinutes();
const hour = now.getHours();
```

再接著去計算應該轉的角度，這裡就是數學問題了

* 秒: 
    一圈60秒，所以用現在的秒數先除以 60，再乘上一圈的度數 360
    secondsDegrees = ((seconds / 60) * 360)
* 分: 
    一圈60分，所以用現在的分鐘數先除以 60，再乘上一圈的度數 360，還要加上秒數的影響。
    一秒走 1/60 分鐘，所以角度是 1/60 * 360 = 6度
    也就是說，分鐘等於 minsDegrees = ( mins / 60 ) * 360 + ( seconds / 60 ) * 6
* 秒: 
    一圈60秒，所以用現在的秒數先除以 60，再乘上一圈的度數 360，同理要加上分鐘的影響
    角度是 1/12 * 360 = 30度 (因為鐘面上是12小時)
    hourDegrees = ( hour / 12 ) * 360 + ( mins / 60 ) * 30

再利用這些算出來的角度來改變指針的 transform

```js
    const now = new Date();

    const seconds = now.getSeconds();
    const secondsDegrees = ((seconds / 60) * 360);
    secondHand.style.transform = `rotate(${secondsDegrees}deg)`;

    const mins = now.getMinutes();
    const minsDegrees = ((mins / 60) * 360) + ((seconds/60)*6);
    minsHand.style.transform = `rotate(${minsDegrees}deg)`;

    const hour = now.getHours();
    const hourDegrees = ((hour / 12) * 360) + ((mins/60)*30);
    hourHand.style.transform = `rotate(${hourDegrees}deg)`;
```

這樣就還差最後一步，因為會發現指針會從指針的中心點開始旋轉，而不是從時鐘的中心點。因此必須在一開始就讓他先偏移 90 度，相對應的剛剛計算的角度也要都再加上 90 度

```css
    .hand {
      width:50%;
      height:6px;
      background:black;
      position: absolute;
      top:50%;
      transform-origin: 100%;
      transform: rotate(90deg);
      transition: all 0.05s;
      transition-timing-function: cubic-bezier(0.1, 2.7, 0.58, 1);
    }
```

```js
  function setDate() {
    const now = new Date();

    const seconds = now.getSeconds();
    const secondsDegrees = ((seconds / 60) * 360) + 90;
    secondHand.style.transform = `rotate(${secondsDegrees}deg)`;

    const mins = now.getMinutes();
    const minsDegrees = ((mins / 60) * 360) + ((seconds/60)*6) + 90;
    minsHand.style.transform = `rotate(${minsDegrees}deg)`;

    const hour = now.getHours();
    const hourDegrees = ((hour / 12) * 360) + ((mins/60)*30) + 90;
    hourHand.style.transform = `rotate(${hourDegrees}deg)`;
  }
```

最後利用 setInterval 設定每秒執行一次 & 一開始的預設執行就可以了

 ```js
setInterval(setDate, 1000);
setDate();
```