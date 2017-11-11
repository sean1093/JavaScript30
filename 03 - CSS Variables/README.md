# 03 - CSS Variables

## 目標

* 利用 JavaScript 來更新 CSS 變數，達到圖片控制的效果 

* DEMO: http://sean1093.github.io/JavaScript30/03%20-%20CSS%20Variables/index-SEAN.html


## 實作

這篇主要目標是要學習如何使用最新 CSS3 變數的用法

第一個重點就是如何使用 CSS3 的變數:
* :root 命名變數，我們也可以在任意的選擇器內命名變數
* --base 是變數名字，#ffc600 是變數值。

我們先定義三個變數

```css
    :root {
        --base: #ffc600;
        --spacing: 10px;
        --blur: 10px;
    }
```

在圖片與標題上套用，要使用變數的時候只要在 var() 內使用就可以了

```css
    img {
        padding: var(--spacing);
        background: var(--base);
        filter: blur(var(--blur));
    }

    .hl {
        color: var(--base);
    }
```


接著利用 JavaScript 去偵測 DOM 的改變，並且進一步去更新 CSS 變數值就完成這篇的目標了

首先一樣先去抓取我們 DOM 上面的元素，並且把每個元素都加上 event listener

```js
    const inputs = document.querySelectorAll('.controls input');

    inputs.forEach(input => input.addEventListener('change', handleUpdate));

    // 這裡要加上 mousemove 的原因，是因為想讓他有連續的感覺，一邊拖曳會一邊改變
    inputs.forEach(input => input.addEventListener('mousemove', handleUpdate));

```

最後來實作 handleUpdate 方法

1. 利用 style.setProperty 來更新特定名稱的 css property
2. 這裡的 suffix 目的是要去抓單位值，透過 'data-*' 屬性去抓我們預先定義在 input 裡面的值

```js
    function handleUpdate() {
        const suffix = this.dataset.sizing || '';
        document.documentElement.style.setProperty(`--${this.name}`, this.value + suffix);
    }
```



## 相關觀念

CSS3 新增了變數的屬性，讓在 CSS 中也能像平常在寫 SASS 或 LESS 一樣，將重覆的參數，直接用一個名稱代替
* 參考: 
* https://developer.mozilla.org/zh-TW/docs/Web/CSS/:root
* http://muki.tw/tech/native-css-variables/


CSS3 提供了濾鏡效果 filter
* blur(): 高斯模糊
* 參考: https://developer.mozilla.org/zh-TW/docs/Web/CSS/filter
