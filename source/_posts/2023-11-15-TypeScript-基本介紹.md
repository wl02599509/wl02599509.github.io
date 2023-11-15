---
title: TypeScript 基本介紹
date: 2023-11-15 23:00:53
tags:
  - TypeScript
---
# 什麼是 TypeScript?

TypeScript 是一個 JavaScript 的延伸工具，我們從它的名字 **Type** 就可以知道，它主要是想要解決 JavaScript 本身屬於弱型別的問題。

# 什麼是弱型別? 弱型別又有什麼問題?

首先，弱型別的意思就是，程式碼在運行時，對型別的認定不會有強硬的要求，且在進行運算時，也會廣納型別，不會因為型別之間的差異，而產生錯誤，如下範例:

<!-- more -->
```jsx
const amountCounter = (a, b) => {
  return a + b
}

1. amountCounter(1, 2) // 3 
2. amountCounter(1, '2') // '12'
3. amountCounter('1', '2') // '12'
```

當我們有一個 function `amountCounter` 負責將兩個引數相加，這時，變數帶入會有上述三種可能，並且產生不一樣的結果。哪個才是這個 function 的預期? 

如果預期是數字相加，像計算機那樣，那第二、三種情況就不如預期; 如果是想把字串串在一起，那第一種情形就不如預期，但是第二個呢?

沒錯，不論是想要數字相加或是字串相加，在怎麼樣，都不應該出現第二個，不過在開發時，一定會發生像是第二種狀況。

由此可知，弱型別的問題在於:

1. 難以在開發時，得知該如何使用資料。就像我無法知道 `amountCounter` 該帶入什麼型別的資料進去。當然透過 function 的名稱，八九成是帶入 number，但 who knows? 又或是 function 運作複雜，名稱中立時，分辨難度便會上升。
2. 在程式碼堆裡，沒有型別的檢查，程式照跑照跳，容易發生使用錯誤型別的資料，產生不如預期的結果，就像上述第二個情況。

只要我們能夠知道所有資料的型別，並且在開發時，能夠得知哪裏有型別上的謬誤，如此便能減少執行後產生的非預期結果，也就是 bug，所以我們可以得出一個結論，可以得出一個結論，JavaScript 是弱型別，TypeScript 的出現是為了解決 JavaScript 本身弱型別所產生的問題。

# TypeScript 如何解決 JavaScript 弱型別的問題

在寫 JS 時，我們只會宣告變數，定義 function，但是 TypeScript 需要額外**宣告型態**:

```tsx
const amountCounter = (a: number, b: number) => {
  return a + b
}

amountCounter(1, 2) // 3
```

將上面的 function 以 TypeScript 撰寫時，必須定義出參數的型別，當參數的型別被定義後，就不能帶入其他型別的資料。

如果不小心帶入非宣告型態的資料時 `amountCounter(1, '2')` ，就會馬上在編輯器上顯示錯誤訊息: `類型 'string' 的引數不可指派給類型 'number' 的參數。` 如此一來，便能馬上知道給錯資料了，因此可以避免在執行時產生非預期結果。而且當我們看到這個 function 的參數已經表明是 `number` 時，在使用上，就顯得更加明確。

# 所以 TypeScript 與 JavaScript 到底是什麼關係

簡單來說，寫 TypeScript 就是寫 JavaScript，TypeScript 本質上就是 JavaScript，寫法都跟 JavaScript 是一致的。因為 TypeScript 本身就是 JavaScript 的延伸。而且，當我們將 TypeScript 寫好之後，仍然會再另外編譯成 JavaScript。

講白話就是，我現在有個 `cat.ts` 的 TypeScript file，當我們執行時，會額外編譯一份 `cat.js` 的 JavaScript file 以供執行。

```tsx
cat.ts

const logo: string = 'Chloe'

console.log(logo)
```

執行 `tsc car.ts` 將 ts file 編譯成 js file:

```jsx
cat.js

const logo = 'Chloe'

console.log(logo)
```

此時執行 `node cat.js` 以執行 js file 得到 `Chloe` 。

由此可知， TypeScript 與 JavaScript 哪裏相同，又有哪裡不相同。

# 總結

經過如此長篇幅的說明，針對 TypeScript 來簡單做個總結。

- TypeScript 是 JavaScript 的延伸工具，一樣是在寫 JS，但要額外宣告型別，以避免 JS 弱型別在開發上延伸出來的問題。
- TypeScript 可以在開發與編譯時，提早發現問題，而不像 JS 等執行後出現非預期結果才知道有問題。