---
title: accept_nested_attributes_for + fields_for 
date: 2022-12-27 22:38:40
tags:
- Rails
- nest_form
---
**accept_nested_attributes_for**
用來讓父 model 的子關聯們的屬性，能被父 model 接受，所以後面接的參數會依照 has_one 或 has_many 決定單複數：
<!-- more -->

```ruby
User model:

has_many: articles, dependent: :destroy
has_one_attached: avatar

accepts_nested_attributes_for :articles, allow_destroy: true
accepts_nested_attributes_for :avatar_attachment, allow_destroy: true
```

通常在使用 anaf 時，只會有 update 不會有 create，因為是透過關聯進行 CRUD，像是：

```ruby
User has_many :articles
Article belongs_to :user

User.articles.new(title: 'Porter', price: 300)
User.update

params.require(:user).permit(articles_attribute: %i[id title price])
```
抑或是：
```ruby
def edit; end

def update
	if current_user.update(permitted_params)
    redirect_to action: :edit, status: :see_other
  else
    render action: :edit
  end
end

def permitted_params
	params.require(:user).permit(articles_attribute: %i[id title price _destroy])
end
```

**fields_for**
巢狀 form 使用。

在上述兩個有關聯性的 model 在使用 anaf 時，表單會使用 `nested_form`  的 `fields_for` 來處理：
```ruby
<%= form_with model: current_user, scope: :user, url: url_for(action: :update), method: :patch do |form| %>
	<%= form.fields_for :articles do |f| %>
		<%= f.hidden_field :id %>   
		<%= f.text_field :title %>
		<%= f.text_field :price %>
	<% end %>
<% end %>
```
表單送出後，打到後端的參數會因為表單為巢狀，而變成巢狀參數：

`params: { ‘user’ ⇒ { ‘articles_attributes’ ⇒ { title: ‘xxx’, price: xxx } } }`

要特別留意，如果是針對 `has_many` 的 model 進行 `nested_form`  時， `fields_for` 會把所有的 model 全部撈出來並 render 出來！ 

此時畫面上會是一個又一個的 article 資料，而每個的 title 跟 price 都會變成 text_field！

如果只希望呈現空白的欄位或是其他種類的 field，可以這樣：
```ruby
<%= form.fields_for :articles, current_user.articles.new do |f| %>
```
這樣就會建立一個 article 物件，且畫面上也只會有一個 title 跟 price 的 text_field，不會有所有 article 的資料。

PS:在 `:article` 後面可以做很多事情。

如果想要 `delete`，則需要有 `_destroy` 且 value 為 `1` 時，就會進行刪除。
```ruby
<%= f.hidden_field :id %>
<%= f.hidden_field :_destroy, value: '1' %>
```
只要有 `:id` `_destroy` 就能夠執行刪除。
進行刪除的方式有很多，像是用 f.checkbox :_destroy 或是另外再給一個 form 讓畫面上有一個刪除鈕：
```ruby
<%= form_with model: current_user, scope: :user, url: url_for(action: :update), method: :patch do |form| %>
	<%= form.fields_for :articles do |f| %>
		<%= f.hidden_field :id %>
		<%= f.hidden_field :_destroy, value: '1' %>
	<% end %>
	<% form.submit '刪除文章' %>
<% end %>
```
以上兩種魔法都蠻好用的也蠻常見的，讓關聯之間的 CRUD 都能變得非常單純。