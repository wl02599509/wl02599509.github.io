---
title: ActiveModel::ForbiddenAttributesError
date: 2022-08-14 20:11:45
categories: Rails
tags:
- Rails
- Problems
---

# 前言

在 Rails 要讓表單將資料送往資料庫時，如果沒有經過「核對欄位」時，就會噴出下圖錯誤：

<!-- more -->

<img>https://images.vocus.cc/2150dd6e-b4ff-4811-a905-3c0b53479845.jpg</img>

---

# WHY?

這是為了避免使用者在資料寫入時，強行新增非官網的資料時，所進行的預設阻擋功能。

# SO~

所以必須新增一個「白名單」，才能正常寫入：

```ruby
params.require(:article).permit(:title, :content)

params.[:article].permit(:title, :content)
```

（兩者都一樣，但業界比較常用第一種寫法。）
寫了這段後，除了 permit 的資料，其他額外的資料都會被過濾掉，不會進來。