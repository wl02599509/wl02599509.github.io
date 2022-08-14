---
title: 搜尋大對決 Find, Find_by, Where
date: 2022-08-14 20:21:25
categories:
- Rails
- Data Base
tags:
- Rails
- SQL
---
# 前言
搜尋的功能非常常見，因為資料不會憑空出現，必須靠我們下條件，撈出符合的資料。那要使用哪個搜尋工具就非常重要了，因為工具間的差異，還是會對搜尋結果造成影響。

<!-- more -->

# Find

輸入一個 id 參數後，尋找相對應的一筆資料。

```ruby
@article = Article.find(params[:id])
```
在 SQL 的部分：

```SQL
Article Load (0.4ms)  SELECT "articles". * FROM "articles" WHERE "articles"."id" = ? LIMIT ? [["id", 3], ["LIMIT", 1]]
```

如果 find 找不到相對應的資料時，會跳出兇殘的錯誤訊息：

https://images.vocus.cc/a79266c7-6ed0-4b39-b94e-0e3215a7ca34.png

此外，find 只能找 id，所以如果要找其他資料，必須透過 find_by 或 where。

---

# Find_by

輸入一個或多個參數後，找到相對應的一筆資料。

```ruby
@article = Article.find(id: params[:id], title: params[:title])
```

在 SQL 的部分：

```SQL
Article Load (0.4ms)  SELECT "articles". * FROM "articles" WHERE "articles"."id" = ? LIMIT ? [["id", 3], ["LIMIT", 1]]
```

如果 find_by 找不到相對應的資料時，會跳出比較溫和的 undefined method 訊息，並回傳 nil：

https://images.vocus.cc/3217ab9b-fc4c-465f-aec6-1c910990d19f.png

在 find_by 後面加驚嘆號，find_by! 也會產生跟 find 一樣的錯誤訊息，但仍能帶入多個參數。

---

# Where

輸入一個或多個參數後，找到相對應的多筆資料。

```ruby
@article = Article.where(params[:id])
```
這個 where 就是 SQL 語法的 where，所以參數也能使用 SQL 語法。

此外，參數也可以是 String / Array / Hash 當搜尋條件。

---

# 比較

## find vs find_by
1. 條件數量不同，find 只能有一個，find_by 可以有很多個。
2. 錯誤訊息不同 （ 但是 find 可以加驚嘆號，就等同 find ）

## find 系列 vs where 系列
1. find 系列只會有一筆資料，where 會找出符合搜尋條件的所有資料
2. where 可以使用 SQL 語法當作搜尋條件

---

### Reference
https://ithelp.ithome.com.tw/articles/10225325