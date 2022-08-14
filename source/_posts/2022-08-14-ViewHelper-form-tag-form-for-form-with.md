---
title: ViewHelper-form_tag/form_for/form_with
date: 2022-08-14 21:01:32
categories: Rails
tags:
- Rails
- View
---

# 前言

在 Rail 的 MVC 架構中，M 是負責驗證與邏輯、以及轉譯向資料庫溝通的角色; C 是負責資料的搬運，搬往 M 或 V; V 則是負責畫面的呈現、及使用者與資料的互動，並將請求送往C，接著得到 C 送回來的資料。

那關於資料的傳輸，一定是要先有個表單，Rails 提供了三個方法，依照需求使用，不過用過之後，通常就會知道哪個最好用。

<!-- more -->
---

# form_tag

```ruby
<%= form_tag url: '/books', method: :post do %>      
　　<input type="text">     
　　<input type="submit">  
<% end %>
```

form_tag 後面接續的是，這份表單按下送出後，要以什麼方法（post），往哪個地方送（‘/books’）。

在 block 裡面則是要自己刻出 html 的 input 標籤。

透過檢視原始碼可以知道，這些 view helper 幫你做了什麼事情，我們來看看 form_tag 在瀏覽器上變成什麼：

```html
<form action="/posts/1" accept-charset="UTF-8" method="post"><input type="hidden" name="_method" value="put" autocomplete="off" /><input type="hidden" name="authenticity_token" value="L4E9KoJ8Cd9YdzAlTvMsKo4it45PXdt4wt0h-M3b6YVtpAAawmncLwU5zgohWiJ2Ke-6obJKCiD5D824Jyjt-g" autocomplete="off" />   
　　<input type="text">   
　　<input type="submit">  
</form>
```

是不是很厲害，短短的 form_tag，幫你做了這麼多事情。其中第二個隱藏欄位，authenticity_token，就是使用 view helper 一大原因，它會幫你做出一個自動產生 authenticity_token 的隱藏欄位，value 後面那一大串就是這個欄位的產物。

（如果你按下重新整理，他就會在跳一組新的）

# form_for

更厲害更方便的 view helper！

在 form_for 後面接上 controller 建立的實體變數，form_for 會去判斷這個實體變數，有沒有東西，並以此決定，這份表單的 action 跟 method 為何。

HOW? 因為整體的過程是這樣：當我們在網頁上按下新增功能時，會往 /books/new 的網址去找 BooksController 的 new action ，new action 會去找 new.html.erb，所以在 new.html.erb 裡的 form_for 的實體變數，是從 new action 來的，由於是新增，資料庫裡沒有東西，所以 form_for 就知道，我現在是在新增一個新的物件，那我應該是要送往 create。

因此那個 form.submit 會是 create 字樣。

所以同樣的 form_for 換到 edit.html.erb 裡，在 edit 的頁面上，submit 鈕顯示的字樣是 update，因為 @book 有抓到東西是從資料庫過來的，那代表我們是要從既有資料進行更新。

```ruby
<%= form_for @book do |f| %>

    <%= form.label :title %>
    <%= form.text_field :title %>
    
    <%= form.submit %>

<% end %>
```

此外，form_for 還有附加功能是 form.labe 跟 form.text_field 等等的工具可以使用，非常方便。

我們來看看網頁原始碼，form 幫我們做了什麼：

```html
<label for="book_title">Title</label>
<input type="text" name="book[title]" id="book_title" />

<input type="submit" name="commit" value="Create Book" data-disable-with="Create Book" />
```

是不是超厲害！

我們輸入的 form，在瀏覽器上轉換成 html 的標籤、屬性、值。

---

# form_with

form_with 則是上面兩個的混血兒，直接來看 code：

```ruby
<%= form_with model: @book, url: '/store' do |f| %>          
　　<%= f.label :title %>     
　　<%= f.text_field :title %>      
　　<%= f.submit %> 
<% end %>
```

有 form_for 的實體變數，也有 form_tag 的 url，當表單要送往的地方不如預期時，可以使用 form_with 自訂 url，讓表單送往我們想要它去的地方，以防它自己找錯路。

---

# 小結

這篇介紹了 MVC 裡，V 會使用到的表單語法，個人比較常用到的是 form_for 跟 form_with。

form_tag 還要自己打 html，對我來說沒有很方便，form_for 幾乎能代替它，但如果需要 url，再用 form_with 就好。

至於上面提到的 authenticity token 是什麼？我們改天再來寫一篇介紹一下。
