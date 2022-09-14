---
title: IT 邦鐵人賽 Day 15 - RSpec 語法 shared_example
date: 2022-09-15 01:27:31
category: iThome
tags: 
- iThome
- Test Automation
- Ruby
- Rspec
---
昨天介紹了整理 code 小幫手 context，幫我們組織測試內容，提高可讀性，今天我們也來認識一下其他節省空間的工具人們。

# Shared_examples
如果我有好幾個受測對象，需要測試的項目都一樣，比方說，有好幾個不同的陣列，我想知道，每個陣列裡是否含有某一個元素，勢必會看到一堆一模一樣的 examples：

<!--more-->

```ruby
RSpec.describe Array do
  subject {[1, 2, true, "c", "ruby"]}
  it "should have 1" do
    expect(subject.include?(1)).to eq(true)
  end
 
  subject {["Rails", :apple, 88, 1]}
  it "should have 1" do
    expect(subject.include?(1)).to eq(true)
  end
 
  subject {["true", false, 1, "c"]}
  it "should have 1" do
    expect(subject.include?(1)).to eq(true)
  end
end
```

因此，我們可以把重複的 examples 抽離，並在需要 example 時， include_examples：

```ruby
RSpec.shared_examples "same_element" do
  it "should have 1" do
    expect(subject.include?(1)).to eq(true)
  end
end
 
RSpec.describe Array do
  subject {[1, 2, true, "c", "ruby"]}
  include_examples "same_element"
 
  subject {["Rails", :apple, 88, 1]}
  include_examples "same_element"
 
  subject {["true", false, 1, "c"]}
  include_examples "same_element"
end
```

在一個比較活的例子，當我想要測試使用者是不是有滿 18 歲時 (不要什麼東西要滿18歲)：

```ruby
class User 
  attr_accessor :age
  def initialize(name, age)
    @name = name
    @age = age
  end
end

RSpec.shared_examples "滿18歲" do
  it "使用者年紀大於等於 18" do
    expect(user.age).to be >= 18
  end
end

RSpec.describe Array do
  let(:user) { User.new('Mike', 20) }
  include_examples "滿18歲"

  let(:user) { User.new('Elly', 18) }
  include_examples "滿18歲"

  let(:user) { User.new('Yummy', 19) }
  include_examples "滿18歲"
end
```

非常方便！

如此一來，只要知道 “same_element” 是什麼，就可以知道每個 examples 在幹嘛了，不像原本要讀整句。

# 小結
今天介紹的 shared_examples 主要就是整理型工具，讓程式碼更有結構，提升閱讀性。

shared_examples 是將重複的 examples 打包並抽離出來，提供其他 it group 使用。

明天我們來介紹他的雙胞胎兄弟: share_context。我想從字面上來看應該很清楚他是在幹嘛的，不過我還是要 po，不然沒辦法完賽XD
