---
title: TypeScript 型態宣告
date: 2023-11-15 23:02:58
tags:
  - TypeScript
---
# 前言

TypeScript 是 JavaScript 的超集，用來補足 JS 在開發中，因弱型別所造成的問題。在 TypeScript 裡，寫的就是 JS，只是我們需要多做型態宣告這件事，讓所有的資料知道自己的身份，才不會像成龍一樣大喊我是誰。今天就來講講該如何進行型態的宣告。

# 變數

在程式的世界裡，宣告一個變數是基本中的基本。我們運用變數來操作與製造我們需要的資料。宣告變數時，我們會給他各種形態的資料，像是字串、數字、布林值、陣列、物件。

在 JS 中，我們可以很輕易的就宣告共種型態的變數:

<!--more-->

```jsx
const name = 'Jay Chou'
const age = 45
const getMarried = true
const song_list = ['擱淺', '告白氣球', '愛到無命不知驚']
const singer = {
  name: 'Jay Chou',
	age: 45,
  getMarried: true,
	song_list: ['擱淺', '告白氣球', '愛到無命不知驚']
}
```

在 TS 中，我們除了宣告變數之虞，還要宣告型態，讓變數本身知道自己到底是屬於哪個世界的人。

## 字串、數字、布林值、undefined、null、any

前三項型態算是最單純且常見的宣告，我們直接看範例:

```tsx
const name: string = 'Jay Chou' // 宣告變數 name 的型態為 string
const age: number = 45 // 宣告變數 age 的型態為 number
const getMarried: boolean = true // 宣告變數 getMarried 的型態為 boolean
```

我們可能很少會在寫 JS 時，定義一個值為 `undefined` 或是 `null` 的變數，但這兩種型態確實是會出現的，所以有定義的必要:

```tsx
const foo: undefined = undefined
const bar: null = null
```

最後的 `any` 非常好用(誤)，就它字面之意，就是任意型態，也就是說當變數宣告了 `any` 型態，該變數就變成百變怪一樣，可以是任何神奇寶貝:

```tsx
const whatever: any = 'abc'
```

## 陣列

我們在陣列裡，會儲放好幾個同型態的資料，所以我們要宣告這個陣列裡的資料是什麼型態:

```tsx
const song_list: string[] = ['擱淺', '告白氣球', '愛到無命不知驚']
const body_secret: number[] = [44, 173, 15]
const truth: boolean[] = [true, false, true, false]
const foo: undefined[] = [undefined, undefined]
const bar: null[] = [null, null]

或是

const song_list: Array<string> = ['擱淺', '告白氣球', '愛到無命不知驚']
const body_secret: Array<number> = [44, 173, 15]
const truth: Array<boolean> = [true, false, true, false]
const foo: Array<undefined> = [undefined, undefined]
const bar: Array<null> = [null, null]
```

## 物件

物件的型態宣告就比上面的還要麻煩。物件本身的結構，可以算是一個小介面。當我們要定義一個物件的型態時，會透過 `interface` 或 `type` 來定義物件的型態:

首先，宣告 Singer 型態:

```tsx
interface Singer {
  name: string
	age: number
	getMarried: boolean
	song_list: string[]
}

或

type Singer = {
  name: string
	age: number
	getMarried: boolean
	song_list: string[]
}
```

再來把宣告好的 Singer 型態，宣告到變數上:

```tsx
const JayChou: Singer = {
	company: 'jvrmusic',
	age: 45,
  getMarried: true,
	song_list: ['擱淺', '告白氣球', '愛到無命不知驚']
}

const JJ: Singer = {
	company: 'jjlin',
	age: 42,
  getMarried: false,
	song_list: ['小酒窩', '愛笑的眼睛', '豆漿油條']
}
```

如此一來，只要戴上了 Singer 型態的帽子，該物件就必須要依照該型態去定義屬性與值，缺一不可:

```tsx
const Matzka: Singer = {
	company: null,
  getMarried: false,
	song_list: ['水災', '一朵花', '大叔Uncle']
} --> company **的型態錯誤，age 屬性沒有定義**
```

### 什麼? 不想透露年紀?

也沒關係，或許 `Matzka` 本人不希望被問「[大叔 你今年貴庚](https://www.youtube.com/watch?v=FVAsGIj5ccY)」，那我們就在 Singer 的型態裡，把 age 改為非必要屬性:

```tsx
interface Singer {
  name: string
	age?: number
	getMarried: boolean
	song_list: string[]
}

或

type Singer = {
  name: string
	age?: number
	getMarried: boolean
	song_list: string[]
}
```

像這樣在定義的屬性 `age` 後面加上 `?` 就代表，該屬性不一定需要，如此一來 `Matzka` 就不需要告訴大家年紀了:

```tsx
const Matzka: Singer = {
	company: "I don't know",
  getMarried: false,
	song_list: ['水災', '一朵花', '大叔Uncle']
}
```

### interface 跟 type 有什麼差別呢?

兩者最大的差別在於嚴謹程度: `interface` 可以彈性擴充， `type` 則是比較死板、裡面定義了什麼就是什麼、不能再擴充。

至於這裡指的擴充，之後讓我們再推出一篇文章為大家介紹介紹。

# 陣列中物件

上面介紹的陣列與物件的宣告都算是單純情況。當我們陣列中要放物件時，就必須要把物件的型態定義出來，然後再針對陣列定義陣列的型態:

```tsx
// 物件本身的型態
interface Singer {
  name: string
	age: number
	getMarried: boolean
	song_list: string[]
}

// 陣列的型態: 我的陣列裡面的資料型態是物件，該物件的型態是 Singer
const singers: Singer[] = [
	{
		company: 'jvrmusic',
		age: 45,
	  getMarried: true,
		song_list: ['擱淺', '告白氣球', '愛到無命不知驚']
	},
	{
		company: 'jjlin',
		age: 42,
	  getMarried: false,
		song_list: ['小酒窩', '愛笑的眼睛', '豆漿油條']
	}
]

或

const singers: Array<Singer> = [ ...略 ] 
```

# 總結

今天介紹的內容是 TS 最基本也是最主要的。我們在宣告變數時，都必須發給他們一張身分證 (宣告型態)，讓變數知道自己屬於什麼。

這有利於開發者在開發時，可以在眾多變數中，點兵點將時，知道該找誰用誰，而不會拿了一個普隆貢，導致開發產生問題。

這篇算是一個 TS 的基礎，其實在宣告型態時，有時我們會給與多個型態，也就是變數可以既是字串又是數字，就像我們既可以是台灣人又可以是美國人，只要沒有要選總統的話。

此外，還有很多較進階的 TS 技術，如果有興趣的朋友，可以再研究研究。