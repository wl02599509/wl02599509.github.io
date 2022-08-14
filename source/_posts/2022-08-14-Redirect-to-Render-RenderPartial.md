---
title: Redirect_to / Render / Render Partial
date: 2022-08-14 20:37:54
categories: Rails
tags:
- Rails
- Render
- View
---
# 前言

在 Rails 裡常常可以看到 redirect_to 與 render，前者是用來轉址，後者則是用來渲染畫面，這樣聽起來，根本上的就差異很大了呀，為何還需要拿來比較？

沒錯，他們在根本上的差異是不同的，但如果我們將不同的方法使用在相同的畫面上，這其中的細節，就耐人尋味了。
<!-- more -->
---

# Redirect_to

redirect_to 後面接網址路徑，是用來轉到該網址路徑去，也就是換到一個新的頁面。

```ruby
redirect_to 'https://www.google.com/'
```

如果在 new 的頁面上:

```ruby
if xxx.save
   (略) 
else
  redirect_to 'new_xxxx_path'
end
```

針對 save false 時執行轉址，會導致在 new 頁面上原本填寫好的資料，全部都消失不見。

因為他是從原本的 A 頁轉到新的 A 頁，所以在原本的 A 頁填好的資料理所當然就不見了。

**（同一條溪流，這一秒的水流跟下一秒的水流是不一樣的水流）**

---

# Render

只要使用 render 就能解決上述的狀況！

Render 是渲染的意思。

也就是說，render 會渲染指定的畫面，渲染的意思就是原封不動的重新呈現，原本有什麼就給你看什麼。

如果我們把 redirect_to 轉址，改成 render 渲染：

```ruby
if xxx.save
   (略) 
else
  render :new
end
```

他就會去渲染 new.html.erb，那只要你在 new 的 view template 畫面上有什麼就渲染什麼，輸入在表格上的資料就不會消失了。

因為不像轉址是到一個全新的頁面，既然是全新，那當然就不會有原本輸入好的資料囉。

---

# Render Partial

Render partial 稱為局部渲染，是 render 的其他功能，它可以把每個內容重複的 view template html.erb 進行整合，將重複的內容，整理到一份命名規則為 _xxxx.html.erb 的檔案裡（第一個底線必備）。

通常在 Rails 裡，new 跟 edit 的 html.erb 會有一樣的表格：

```ruby
/views/article/new.html.erb

<h1>新增</h1>
<%= form_for @article do |form| %>
    ....(略)
<% end %>
```

```ruby
/views/article/edit.html.erb

<h1>編輯</h1>
<%= form_for @article do |form| %>
    ....(略)
<% end %>
```

我們可以把它們重複的部分（表格），整理到 **_form.html.erb** 然後讓 **new.html.erb** 跟 **edit.html.erb** 去渲染 **_form.html.erb**：

```ruby
/views/article/_form.html.erb

<%= form_for @article do |form| %>
    ....(略)
<% end %>
```

```ruby
/views/article/new.html.erb

<h1>新增</h1>
<%= render 'form' %>
```

```ruby
/views/article/edit.html.erb

<h1>編輯</h1>    
<%= render 'form' %>
```

如此一來便符合 Ruby 的原則：避免重複。

---

# 為何稱為局部渲染？

從上面的範例可以知道，new 跟 edit 並不是把自己所有的畫面都渲染了，而是保留了各自不同的部分：

```html
<h1>新增</h1>
<h1>編輯</h1>
```

並渲染了局部重複的部分：

```ruby
<%= form_for @article do |form| %>
    ....(略)
<% end %>
```

# Render Partial 彈性大增

在 render partial 的功能裡，還能夠替每個需要的 html.erb 進行渲染客製化，增加渲染的彈性。

```ruby
/controller/articles_controller.rb

def create
    @article = Article.new(article_params)
    (略)
end
```

```ruby
/views/article/_form.html.erb

<%= form_for article do |form| %>
    ....(略)
<% end %>
```

```ruby
/views/article/new.html.erb

<h1>新增</h1>
<%= render 'form', article = @article %>
```

```ruby
/views/article/edit.html.erb

<h1>編輯</h1>   
<%= render 'form', article = @article %>
```

將 render 後面多加一個區域變數，並讓實體變數指向該區域變數，用這個實體變數指向區域變數的方法，好讓每個局部渲染的畫面，都能再另外客製化各自需要的東西。

在 class 裡的 action 底下製作需要的實體變數，然後指向區域變數。
