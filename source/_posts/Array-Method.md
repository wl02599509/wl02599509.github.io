---
title: Array_Method
date: 2022-08-14 15:52:33
tags: Ruby, Array
---
在學習程式語言時，操縱陣列是非常頻繁的一件事情，因為資料的貯存與提取，不外乎就是 Array, Hash 的型態來處理。

本篇將整理陣列中常使用的 Methods，也算是筆記整理的一個篇章。
<!-- more -->

# 基本常用
```ruby
arr = ["a", "b", "c", "D", "E", "F"]
p arr[2..4] #印出陣列中第三順位至第五順位，並組成陣列
p arr[2, 3] #取出陣列裡第三順位起算的三個物件，組成陣列
p arr[2..-4]
p arr.at(-2) #印出指定順位之物件
p arr.take(3) #取出以第一筆為首指定數量之物件並組成陣列
p arr.drop(1) #去掉前n個物件後，組成陣列
p arr.empty?  #陣列是否空
p arr.include?("D") #陣列中是否包含某物件
p arr.sample(2) #從陣列中隨機回傳兩個物件
p arr.shuffle #回傳一個重新隨機排列的陣列
p arr.sort #將數字陣列按大小順序排列
```
---

# 增加系列
```ruby
arr.push([78, 24, 33]) #放一個物件到陣列最後面
arr << 100 #放一個物件到陣列最後面
p arr.insert(3, "ELLY") #於指定順位放入特定物件
p arr.unshift("9") #插一個物件到陣列第一序位
```

---

# 減少系列
```ruby
p arr.pop #移除最後一個物件並回傳，此時arr就少了它
p arr.shift #移除第一個物件並回傳，此時arr就少了它
p arr.delete_at(3)  #移除指定序位的物件
p arr.delete("b") #移除指定物件，並全部刪除
arr = [1, 2, 7, nil, 4, 4, 0]
p arr.compact #將陣列中的nil值移除
p arr.uniq #將陣列中重複的物件移除
```

---

# .Combination method
```ruby
arr = [1, 3, 9, 7, 2]
arr.combination(3) { |x| p x} 
#從頭按順序挑出三個物件排成一個陣列的所有可能陣列
#參數為0，則輸出[]
#參數若為負數或超出範圍，則不會丟入BLOCK裡
```

---

# .intersection 及 &
```ruby
#可將多個陣列共同擁有的物件取出後組成一個新陣列
arr = [1, 2, 3, 4, 5] 
Ar = [5, 3, 1, 7, 9]
x = ["a", "E", 5, "Box", "sis"]
y = ["E", "sis", "a"]
z = [1, 5, "E", "Box", 2]
p arr & Ar
p x & y
p x & y & z
p arr.intersection(Ar)
```

---

# 計算長度
```ruby
.length  .count  .size
#計算陣列中的物件有多少個
arr = [0, "a", :XD, [1, 3, 5],7]
p arr.size
```

---

# 陣列 * n 會產生一組n份的陣列
```ruby
arr = [0, "BOY", "下雨", :weather]
p arr * 3
```

---

# 陣列們彼此相加
```ruby
x = ["a", 1, 5, "耶"]
y = ["C", "a", 1]
p x + y
```

---

# 會破壞原陣列的選擇
```ruby
arr = [ 1, 2, 3, 4, 5, 6, 7, 8, 9]
p arr.delete_if { |a| a < 3 } #接下來的指令所用的陣列就會改變成這個
p arr.keep_if { |a| a < 7 } #此執行的陣列為上面留下來的陣列
```

---

# .rotate method
```ruby
#讓陣列以引數+1的序位為首依序排列
arr = [ 1, 11, 4, 3, 16, 0]
p arr.rotate(0)  =>  [ 1, 11, 4, 3, 16, 0]
p arr.rotate(1)  => [ 11, 4, 3, 16, 0, 1]
p arr.rotate(2)  => [ 4, 3, 16, 0, 1, 11]
```

---

# .split(//) 
```ruby
#可以把一個字串拆解成字元字串，並變成陣列
num = 12345    
arr = num.to_s.to_split(//) 
p arr  =>  ["1", "2", "3", "4", "5"]
```