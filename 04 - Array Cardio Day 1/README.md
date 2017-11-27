# 04 - Array Cardio Day 1

## 目標

* 學習 JavaScript array 的基本操作

* DEMO: 

## 觀念

### filter

從 array 中找出回傳 true 的 element

ex: 找出陣列中小於 5 的數字

```js
const newArray = [2,6,10,9,3,7,5,4,-1].filter((e) => {
    if (e < 5) return true;
    else return false;
})
// newArray = [2, 3, 4, -1]
```

### map

對 array 中所有 element 處理過後再回傳新的 array

ex: 把所有陣列中大於 5 的數字減 5，小於等於 5 的數字加 5

```js
const newArray = [2,5,7,9].map((e) => {
    if (e > 5) return e - 5;
    else return e + 5;
})
// newArray = [7, 10, 2, 4]
```

### sort

預設的 sort 陣列會根據各個字元的 Unicode 碼位值排列，或根據每個元素轉換為字串，所以會出現 11 會排在 2 前面這種情況，因此我們通常必須加上 compareFunction

ex: 一些在 https://developer.mozilla.org 上的經典例子

```js
let fruit = ['apples', 'bananas', 'Cherries'];
fruit.sort(); // ['Cherries', 'apples', 'bananas'];

let scores = [1, 2, 10, 21]; 
scores.sort(); // [1, 10, 2, 21]

let things = ['word', 'Word', '1 Word', '2 Words'];
things.sort(); // ['1 Word', '2 Words', 'Word', 'word']

// 在Unicode中, 數字在大寫字母前,
// 大寫字母在小寫字母前
```

#### compareFunction

* 觀念:
1. compareFunction(a, b) return 小於 0, 將 a 排在比 b index 還小處
2. compareFunction(a, b) return 0, a 與 b 互相不會改變順序
3. compareFunction(a, b) return 大於 0, 將 b 排在比 a index 還小處

最常見的例子就是實作 sort by number

```js
const array = [8,10,3,6,1,39];
array.sort();
// ==> [1, 10, 3, 39, 6, 8]

const array2 = [8,10,3,6,1,39];
// 這裡利用 a-b ，如果 a < b 這樣就會回傳負數值，a 就會被排在比 b index 還小處
array2.sort((a,b)=> a-b)
// ==> [1, 3, 6, 8, 10, 39]

```

### reduce

這是一個可以作為累加器的 function

```js
Array.prototype.reduce(
  (accumulator, currentValue, currentIndex, array) => {
    return accumulator + currentValue;
  },
  initValue
);

// example

[0, 1, 2, 3, 4].reduce( (prev, curr) => prev + curr ); // 10

[0, 1, 2, 3, 4].reduce( (prev, curr) => prev + curr, 10 ); // 20

```