---
title: IT 邦鐵人賽 Day 11 - RSpec 語法 before_hook & after_hook
date: 2022-09-11 00:07:14
category: iThome
tags: 
- iThome
- Test Automation
- Ruby
- Rspec
---
在前一篇，我們介紹了 RSpec 裡基礎的語法，今天我們來介紹很好用很常用的語法：before hook, after hook。

當我們需要重複測試同一個物件的不同屬性或方法時：

<!--more-->

```ruby
class Hero
  attr_accessor :damage, :skill
  def initialize(damage, skill)
    @damage = damage
    @skill = skill
  end
end
 
RSpec.describe Hero do
  it "cause 76 damage" do
    @ashe = Hero.new(76, 'ice_arrow')
    expect(@ashe.damage).to eq(76)
  end
 
  it "can use skill" do
    @ashe = Hero.new(76, 'ice_arrow')
    expect(ashe.skill).to eq('ice_arrow')
  end
end
```

我們在測試建立英雄時，分別測試他的傷害跟技能前，都必須 Hero.new 出一個物件，這樣子太重複太囉唆了。所以如果可以一次完成所有 examples 都需要的 Hero.new 那該有多好。

於是乎，RSpec 聽到了大家的渴望：

# before hook

這個語法其實只有 before 沒有 hook，hook 是鉤子、掛鉤的意思，所以 before hook 就是在某個點之前先完成某些事情，也就是在 it 之前，先完成你想要他完成的事情：

```ruby
RSpec.describe Hero do
  before do
    @ashe = Hero.new(76, 'ice_arrow')
  end
    it "cause 76 damage" do
    expect(@ashe.damage).to eq(76)
  end
 
  it "can use skill" do
    expect(@ashe.skill).to eq('ice_arrow')
  end
end
```

像這樣，把要做的事情放到 before 的 block 裡，如此一來，他就會在每一個 example 執行前，先執行一次 before 的 block 裡的事情，這樣只要寫一個 before 就能取代掉很多重複的行為。

但如果你有 18 個 example，其中有 5 個不需要呢？before 一樣也會在那不需要的 5 個 example 執行前，也給你 block 裡面的東西，所以還有一個更好用的方法 let，稍後再介紹給大家。

# after hook

認識了 before hook，相信這個 after hook 就不那麼陌生了(才怪)。

after hook 是在 example 完成之後開始執行。

完事了才發功？沒錯，聽起來不知道能幹嘛，直接來看範例：

```ruby
RSpec.describe "after_hook" do
  before "new arr" do
    @arr = []
    p @arr
  end
 
  after "clear all" do
    @arr.clear
    p @arr
  end
 
  it "hook" do
    @arr << "hook"
    @arr.push("abc")
    @arr.unshift(123)
    expect(@arr.length).to eq(3)
    p @arr
  end
end
```

得到結果：

```ruby
after_hook
[]
[123, "hook", "abc"]
[]
  hook
```

我們在每個步驟都印出 @arr ，第一個空陣列 [ ] 是 before、第二個陣列是在 example 經歷了各種方法後含有各種物件的陣列、第三個空陣列則是 after 的結果，這樣就很清楚 before 跟 after 的執行順序。

感覺好廢喔？其實也不會啦，當一個 example 整個做完後，可能會讓物件變得很龐大，這時可以透過 after 將它清空，或是變回原始的樣子，以此減少效能的佔用。

為 hook 加入參數

沒錯！不要小看 before 跟 after，在一般的情況下，預設是 (:each)：

```ruby
RSpec.describe "before_hook" do
  before(:each) do
    @arr = ["a"]
  end
 
  it "hook" do
    p @arr
    @arr << "b"
    p @arr
  end
 
  it "hook2" do
    @arr << "c"
    p @arr
  end
end
```

得到結果：

```ruby
before_each_hook
["a"]
["a", "b"]
  hook
["a", "c"]
  hook2
```

沒錯，這是在一開始提到的 before，在每個 examples 前都會先執行。

那如果改成 (:all)：

```ruby
before_all_hook
["a"]
["a", "b"]
  hook
["a", "b", "c"]
  hook2
```

他就只會在第一個 example 執行一次 before，接下來的 example 就不會有在執行 before，所以就造成物件一直被沿用下去。

after 也是同樣的道理，預設是 (:each)，可以另外加入其他參數改變它作用的地方。

# 小結

總結一下今天的 hook:

before 是在 run example 之前，先執行 before block 裡的事情，所以可以用來解決重複在每個 example 做同樣的事情。

after 則是在 example 的最後才執行它 block 裡的事情，比較多是用在example 結束後，物件內容的清除。

before 跟 after 都可以加入參數，預設是 (:each)。

明天我們來介紹今天提到的 let。
