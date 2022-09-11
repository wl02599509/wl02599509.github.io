---
title: IT 邦鐵人賽 Day 12 - RSpec 語法 let method
date: 2022-09-12 02:42:35
category: iThome
tags: 
- iThome
- Test Automation
- Ruby
- Rspec
---
昨天介紹了 hook ，其中的 before hook 是為了不要一直做重複的事情，但這樣會造成每個 examples 都會執行，不論 examples 有沒有需要，這樣似乎有點浪費（客家精神發作），今天介紹的 let 是更方便更好用的方法。

<!--more-->

# let method
我們直接來看 code：

```ruby
let(:book) { Book.new("解憂雜貨店", 100, 380) }
```

let 後面接小括號，小括號裏面給一個符號，而這個符號則可以在 examples 裡面當變數使用，但當變數使用時，是不需要冒號的喔！

小括號後面接大括號，大括號裏面接的就是任何的運算，而小括號內的符號會指向大括號內的結果。

如果換成這樣，應該就明白了：

```
@book = Book.new("解憂雜貨店", 100, 380)
```

意思是一樣的，只不過在 examples group 裡，必須使用 let method，如此能讓所有 examples 都能夠使用 article，沒錯，區域變數即可，不需要實體變數。 

像這樣的範例：

```
class Book
  attr_accessor :title, :price, :pages
  def initialize(title, price, pages)
    @title = title
    @price = price
    @pages = pages
  end
end
 
RSpec.describe "let method" do
  let(:book) { Book.new("解憂雜貨店", 100, 380) }
 
  it "costs 100 dollar " do
    expect(book.price).to eq(100)
  end
 
  it "have 380 pages" do
    expect(book.pages).to eq(380)
  end
end
```

就非常清楚。let 直接讓大家都能使用到區域變數 book，真的超方便！

# 與 before hook 比較

同樣的例子改用 before hook 比對一下：

```
RSpec.describe "let method" do
  before do
    @book = Book.new("解憂雜貨店", 100, 380)
  end
 
  it "costs 100 dollar " do
    expect(@book.price).to eq(100)
  end
 
  it "have 380 pages" do
    expect(@book.pages).to eq(380)
  end
end
```

看起來差沒多少，頂多 examples 裡的 book 要多一個 ＠ 變成實體變數，但我個人比較喜歡 let，不用打 ＠，然後也可以存起來放，想用就用，不像 before，不想用還逼我用。


# 小結
今天的介紹的 let 算是使用度與實用性高的方法，而且也簡單好懂，讓人一用就愛上，明天我們來介紹另一個特別的方法。