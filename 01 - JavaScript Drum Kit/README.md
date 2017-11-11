# 01 - JavaScript Drum Kit

## 目標

* 利用敲擊按鍵來發出不同的鼓聲

* 按下按鍵後，畫面上的字母方塊會有動畫

* DEMO: http://sean1093.github.io/JavaScript30/01%20-%20JavaScript%20Drum%20Kit/index-SEAN.html

## 實作

首先必須要註冊一個 keydown 事件來偵測按鍵被按下

```js
window.addEventListener('keydown', playSound);
```

按下去執行 playSound 這個方法
這個方法主要要做三件事：
1. 分辨按下的按鍵是哪個
2. 利用 querySelector 抓到對應的 audio tag 播放音效
3. 利用 querySelector 抓到對應的 div 字母方塊產生動畫

```js
// 1. 按下的按鍵可以用 e.keyCode 來看他的 code

// 2. 利用 querySelector 抓到對應的 audio tag 播放音效
    // 抓取 audio 中 attribute 為自訂 data-key 且等於 e.keyCode 的元素
    const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
    // 假如沒抓到 (不是我們 target 的按鍵) 就不要做事
    if (!audio) return;
    // 為了可以重複播放，每次播放之前，設置播放時間為 0
    audio.currentTime = 0;
    // 播放
    audio.play();

// 3. 利用 querySelector 抓到對應的 div 字母方塊產生動畫
    // 抓取 div 中 attribute 為自訂 data-key 且等於 e.keyCode 的元素
    const key = document.querySelector(`div[data-key="${e.keyCode}"]`);
    // css 中已經定義 playing 這個 class 為按鍵方塊動畫，因此只要加上去就可以了
    key.classList.add('playing');
```

接著會發現，按下去後動畫不會消失，因此必須再加上另一個 Listener

```js
  //得到所有 class = key 的元素集合並回傳陣列
  const keys = Array.from(document.querySelectorAll('.key'));

   // transitionend 事件會在 CSS transition 结束後觸發
  keys.forEach(key => key.addEventListener('transitionend', removeTransition));

```

在 removeTransition 這個事件裡，主要要做的就是把剛剛的 class 給移除掉

```js
  function removeTransition(e) {
        // 這裡多加一段來檢查，這個 propertyName 有沒有動畫的 transform (定義在 playing class)
        if (e.propertyName !== 'transform') return;
        // 直接 remove class
        e.target.classList.remove('playing');
  }
```

## 相關觀念

* querySelector 抓出來的是一個 NodeList，需要使用 Array 方法進行操作的話可以使用 Array.from(nodeList) 來轉換
