---
title: IT 邦鐵人賽 Day 13 RSpec 語法 subject method
date: 2022-09-14 10:17:37
category: iThome
tags: 
- iThome
- Test Automation
- Ruby
- Rspec
---
寫程式寫到後來發現，最難的其實是「取名」，要取個一眼就知道這是在幹麻的變數名稱，實在是門藝術，像我這種思維方正又收斂的人，取名真的是一大困難，貼心如 RSpec 也聽到了大家的困擾，給大家一個不用取名的方法：

<!--more-->

# subject method

首先，subject 後面可以接一個大括號 { } 裡面放你要的任何東西，直接來看例子：
```
class Book
  attr_accessor :title, :price, :pages
  def initialize(title, price, pages)
    @title = title
    @price = price
    @pages = pages
  end
end
 
RSpec.describe "123" do
  subject{ Book.new("解憂雜貨店", 100, 380) }
 
  it "costs 100 dollar " do
    expect(subject.price).to eq(100)
  end
 
  it "have 380 pages" do
    expect(subject.pages).to eq(380)
  end
end
```

也可以：

```
RSpec.describe "subject demo" do
 subject { "RSpec is the best" }
 
 it "should be 17 elements" do
   expect(subject.length).to eq (17)
 end
 
 it { is_expected.to eq("RSpec is the best") }
end
```

當然還可以放陣列、Hash等等。

看吧！根本不需要取什麼名字，直接 subject 用到底！這個方法就很像 let ：

```
 let(:book) { Book.new("解憂雜貨店", 100, 380) }
```

另外還有一個神奇的地方是，只要 RSpec.describe 後面接的是常數類別，subject 就會直接幫我們 new：

```
RSpec.describe Array do
  it "is an array" do
    subject << "cat"
    subject.push("dog")
    subject.unshift("rabbit")
    p subject
  end
end
```

結果：

```
Array
[]
["rabbit", "cat", "dog"]
  is an array
```

像這樣直接使用 subject 會直接透過 RSpec.describe 後面的常數類別 Array new 出一個空陣列，接著 subject 就可以接陣列的方法。所以它也可以是 Hash, String 等等。

不知道各位剛剛有沒有注意到一段很怪異的 code：

```
it { is_expected.to eq("RSpec is the best") }
```

直接寫起英文來了？不錯！這也是 subject 厲害的地方！只要是用 subject，就可以使用 is_expected 方法，而 expect 這邊的 subject 就可以省略。

```
RSpec.describe "5xRuby" do
 it { is_expected.to eq("5xRuby") }
end
```

用英文的文法來看就好像 it 變成了 subject 的代名詞，讓整體的 readability 提升很多！

視情況使用 subject 真的可以省下很多力氣，也可以避免重複，看起來又乾淨利落。

# 小結
我們來總結一下:

subject 可以變身成 RSpec.describe 後面接的東西。

常數類別：Hash、Array、String、甚至是 class 也可以幫你 new。

其他任何有元素的物件：字串內容 ( “ABCDE” )、數字( 12345 )、陣列( [1, 2, 3] )...等等。

也就是 subject 會指向 RSpec.describe 後面的東西，成為變數。

最猛的部分就是 it 後面可以直接接 is_expected 方法。

這幾天的內容可能都沒太簡單，明天我們來講一個輕鬆的內容讓大家緩緩。
