---
title: 如何取得 Action Text 的 Content 呢？
date: 2022-10-05 11:09:55
category: 
- Rails
- Action Text
---
# 前言
Action Text 是 Rails 提供的魔法之一，它可以讓你的表單的欄位轉變成 trix-editor，如此一來便能在該欄位裡享受寫文章的快感與樂趣，而不像一般的欄位，只能寫到底，無法換行。

但在進行測試，需要驗證trix-editor產出來的值時，卻發生了意外，事情不再是單純的 xxx.content 就能得到它的值了...
<!--more-->

# 問題與解決 (指令環境為 rails console)
假如 **tasks** table 的 content column 使用 action_text 的話，訊息內容就會被存到 **action_text_rich_texts table** 裡，此時 tasks 的 content 欄位必為 nil ( 所以不能對需要使用 action text 的欄位增加 null: fulse 的限制 )，但你輸入 Task.first.content 時，就會顯示 action_text::rich_text 物件。

所以我們要再從 action_text::rich_text model 使用 body 再去找他的內容：
    Task.first.content.body
這時就會獲得 action_text::content，也就是第一個任務欄位的 action_text 模組底下的 content 物件。

## to_s
透過 .to_s 也就是 Task.first.content.body.to_s，可以把這個物件的內容帶出來轉換成字串，顯示的就會是html 字串形式:

```html
"<div class=\"trix-content\">\n  <div>task_real_content_written_by_user<br><br>\n</div>\n</div>\n"   
```

接著就是透過字串 method 把 task_real_content_written_by_user 以外不需要的字 delete 掉就能得到我們所需要的 content，如此一來，測試就能成功抓到它了！

## body_before_type_cast
但這樣處理上來，似乎有點費工，我們可以使用 body_before_type_cast：Task.first.content.body_before_type_cast，它會把上述一大串，還原成還沒被套入 trix-content 的 div 前的樣子：

```html
<div>task_real_content_written_by_user<br><br>\n</div>
```
這樣就超方便啦～ 接著只需要把 <div> <br> \n 這些渣渣刪掉，就能得到測試需要抓到的值囉～