---
title: 遠看是 Trouble，近看是 Turbo
date: 2024-01-05 22:52:30
category: 
  - Rails
  - Turbo

---
# 前言

會想寫一篇 Turbo 主要是近期的專案大量使用 Turbo，且在某項需求的開發過程，讓我必須深入 Turbo，以免讓 Turbo 成為開發上的 Trouble。

# 什麼是 Turbo

Turbo 是 Rails 7 裡佔有一席之地的功能。Rails 7 將 Hotwire 作為預設的前端框架，其中包誇在內的 Turbo 就是本篇主角。

Turbo 主要是想提高畫面上元素的更新效率，以提升使用者體驗，在一般透過導向方式來替換頁面的網頁設計，使用者能明顯地感受到每一次的換頁帶來的停頓，即便不到一秒，但使用 Turbo 則不會有這樣的停頓，讓使用者在操作時倍感順暢。

## 如何運作

假如我們現在在 A 頁面，頁面上有通往 B 頁面的連結。

<!-- more -->

在說明運作之前，我們要先知道，透過 Turbo 所進行的換頁，其實不是換頁，而是仍在同一頁 ( A頁面 )，並改變該頁面上的內容 ( HTML )。

那改變的方式則是將原本的 A 頁面內容 ( HTML ) 抽掉，並放入 B 頁面的內容 ( HTML )，所以我們可以知道 Turbo 是透過一整包的 HTML 的流動來改變原頁面上的內容，而**非前往一個新的頁面**。當 HTML 換好後，最後把瀏覽器上的網址換成從 A 頁面的網址換成 B 頁面的網址。所以我們是一直在同一張紙上，不斷地更換紙上的內容。

### 點擊連結觸發 link

當我們點擊 link 後，會透過 Ajax 向後端發送 request，並在收到 response，將 response 處理完後，將整包 HTML 渲染到新的頁面上。

### 局部更換

除了像換頁一樣換掉整個頁面的內容以外，Turbo 也能只更換一小區塊 HTML，可以說 partial 也可以說 component。

總而言之，除了整張更換以外，也能進行局部更換。

# Turbo 的種類

主要分為 `turbo frame` 與 `turbo stream` ，前者主要是透過 `get` ，後者則是 `get` 以外的 http method，所以如果想要透過 `turbo frame` 去處理 `post` 的請求，它應該是不會理你的，反之。

用法基本上是將 `turbo frame tag` 跟 `turbo stream tag` 掛上 `id` ，並將要替換的 HTML 也取上相同 `id` ，便能完成局部替換。

此外，Turbo 還提供了七種動作，讓開發者決定要用什麼方式來替換。關於詳細實作，可參閱 https://5xruby.com/zh/articles/javascript-hotwire-turbo-partial-load。

至於整頁替換，在 Rails 7 預設使用 Turbo 的情況下，每個 link 都會是以 Turbo 的方式進行所謂的換頁。

# Turbo Event

洋洋灑灑講了一大堆，終於到了本篇要講的重點。會在 Turbo Event 上著墨，主要是因為在開發上遇到的需求與之關係緊密，因此不小心挖掘了 Turbo 的深處。

## 環境

Rails 7，使用 Turbo 進行開發。

## 角色

A 頁面、B 頁面、C 頁面。

A 頁面上有 B、Ｃ頁面的 link。

## 需求情境

在 A 頁面點擊 B、C 頁面的 link 時，必須跳出介面，讓使用者選擇是否離開。

本篇主要探討，點擊 B、C 頁面 link 的處理，後續的介面不包含在討論範圍。

這裡主要處理的問題在於，**當我點擊 B、C 頁面的 link 之後，不能換頁**。

## 解決

### beforeunload

首先可能會想到用 `beforeunload` 事件來 `preventDefault()` 停止事件繼續發生:

```jsx
window.addEventListener('beforeunload', (e) => e.preventDefault())
```

由於 `beforeunload` 是在一般換頁時，會將 document 進行 unload，但 Turbo 並不會，所以在此情況下，`beforeunload` 是無法偵測到由 Turbo 的換頁。

### Turbo Event

沒錯，屬於 Turbo 的鍋，就要由 Turbo 自己來處理。Turbo 也擁有自己的 Event 以供監聽。我們可以在 [Turbo官網上關於 Event 的介紹](https://turbo.hotwired.dev/reference/events) 中看到，Turbo 本身的生命週期。

在眾多 Event 中，我們要挑出可以終止行為的事件，裡面比較合乎需求的 Event:

- `turbo:before-visit`
- `turbo:before-fetch-request`

在這個 case 中，我最後選用 `turbo:before-visit` 來進行實作，待會會解釋為何不適用第二者:

```jsx
document.addEventListener('turbo:before-visit', (event) => {
	event.preventDefault()
})
```

如此一來，便能成功阻止換頁。但這邊要留意，由於綁定在 `document` 上，所以要確認需要作用的範圍，也就是在哪些頁面上需要做這件事情。可以透過 `event.currentTarget.URL` 得知當前的頁面的 `url` 來加以篩選觸發行為的範圍。若希望可以取得該 Turbo 要更新的頁面，可透過 `e.detail.url` 。

## 遇到的困難 - Turbo Cache

在使用 Turbo 時，Turbo 會將以 Turbo 方式 render 的 HTML 緩存成 Cache，以便提升換頁效率。也因為有了 Cache，當使用 `turbo:before-fetch-request` 時，因為雖然有觸發 `event.preventDeafult()` ，但仍然會換頁。

### 情境

一開始是使用 `turbo:before-fetch-request` 開發上述需求，但發現從 B 頁前往 A 頁，A頁點擊 B頁 link 時，理當不該換成 B 頁，卻還是換成 B 頁。

```jsx
document.addEventListener('turbo:before-fetch-request', (event) => {
	event.preventDefault()
})

// 一樣會換頁
```

### 肇事原因

上述程式碼，確實有阻止 `turbo:before-fetch-request` 本身，也就是有執行 `event.preventDefault()` ，但因為有 Cache，所以基本上他根本不需要送出任何 request ，直接拿 Cache 裡的 HTML 來換頁即可，此時在此事件中，雖然阻止了事件，但並沒有辦法防止 Turbo 使用 Cache。

會發現 Turbo cache 的原因在於，當換頁之後，console 上一片空白，沒有任何的 requset 或 query 。

再者 `turbo:before-cache` 事件，我們沒辦法使用 `event.preventDefault()` 來阻止對 Cache 的使用，而且目前 Turbo 的設計，我們沒辦法對 Cache item 進行操作，最多就是使用 `Turbo.cache.clear()` 。

所以最後改用 `turbo:before-visit` 。

### 番外篇

上面的需求解決後，來了更棘手的需求: 按下瀏覽器的上一頁後，也要有同樣的行為 ( 不換頁 )。

因為返回上一頁都是使用 cache 換頁，而且在某些機制上，似乎跟 Turbo 又不太一樣，所以不會觸發 `turbo:before-visit` ，於是使用 `turbo:before-render` 來客製化 render 的 element 製造假象:

```jsx
document.addEventListener('turbo:before-render', (event) => {
	event.detail.render = (currentElement, newElement) => {
    document.body.replaceWith(currentElement)
  }
})
```

複寫 `turbo:before-render` 處理 render 的行為，讓他不換新的內容，一律使用原本的內容，如此便能有尚未換頁的效果。

但這其實華而不實，因為瀏覽器上的 url 已經改變了，而且這有可能會對原本頁面上要做的事情產生影響。

# 結論

整理一下在這次的開發需求上學到的重點:

1. Turbo 有 cache 機制，緩存用 Turbo 來進行交換的 HTML，當 Turbo 訪問相同位置時，Turbo 會**使用 cache** render 頁面，並**不會發送 request**，以提升頁面更新速率。
2. Turbo 的 cache 無法被拿來操作，比方刪掉特定 cache。能做的就是 `Turbo.cache.clear()` 把所有 cahce 刪掉。
3. `turbo:before-fetch-request` 事件主要是在處理**即將發送的實際 HTTP 請求**，跟 cache 的使用是分開的，即便 `turbo:before-fetch-request` 停止了， Turbo 還是會用 cache 去 render 畫面。
4. `turbo:before-visit` 事件主要是在訪問位置之前，因為尚未訪問，所以 Turbo 還不會使用 cache。
5. `turbo:before-render` 事件可以處理**客製化如何換頁**，讓我們自己決定該怎麼改變 render 的方式，而且翻了一下 `source code` 發現，不管是整個頁面或是局部替換，都是透過 `before-render` 處理 render 範圍來實現。