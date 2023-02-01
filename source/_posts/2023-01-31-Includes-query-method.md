---
title: Includes query method
date: 2023-01-31 22:45:36
category: 
  - Rails
  - Active Record Query
tags:
  - includes
  - query
  - n+1
---
N + 1 一直是 rails 在讀取資料的效能殺手，解決這個效能問題有很多方法，今天這篇簡單地講解一下其中一個方法: **includes**。
<!-- more -->

includes: query method，可避免 N+1 問題，針對物件關聯進行資料撈取：
```ruby
ClassRoom has_many :students

ClassRoom.includes(:students)

Student has_one :bag
Student.includes(:bag)
```
此外，還可以用 where 以增加篩選條件來撈取資料：
```ruby
ClassRoom.includes(:students).where('students.name == ?', '王小美')
ClassRoom.includes(:students).where('students.height >= ?', '150')

Student.includes(:bag).where('bags.color = ?', 'blue')
```

這部分要留意的是，where 裡所帶的條件必須為資料表裡存在的 table 與 column。承上，代表有個 table 叫 students，而這個 table 有個欄位叫 name 跟 height。

因為 table 的名稱都是複數，所以在 includes 關聯為一對一用單數時，where 裡的 table 要記得 s 。

若發生錯誤的話，加上 references 就可以正常撈取資料了：
```ruby
ClassRoom.includes(:students).where('students.name == ?', '王小美').references(:students)
ClassRoom.includes(:students).where('students.height >= ?', '150').references(:students)
```