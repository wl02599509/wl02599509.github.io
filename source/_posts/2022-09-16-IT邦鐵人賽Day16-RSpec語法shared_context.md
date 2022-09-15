---
layout: IT 邦鐵人賽 Day 16 - RSpec 語法 shared_context
title: shared_context
date: 2022-09-16 00:51:26
category: iThome
tags: 
- iThome
- Test Automation
- Ruby
- Rspec
---
昨天介紹了 shared_examples 的概念，如果理解，今天的內容就非常容易了。事不宜遲，馬上來介紹我們的 shared_context 吧。

# shared_context

<!--more-->
一樣是在測試項目一樣的情況，shared_examples 主要就是把重複測試項目抽離，而 shared_context 則是把「受測標的」全部抽離打包成一個 group，然後再 include_context 到測試項目的 group 裡：

```ruby
class Comebuy
  attr_accessor :containers, :liquid
 
  def initialize(container, liquid)
    @containers = container
    @liquid = liquid
  end
end
 
RSpec.shared_context "Comebuy" do
  let(:bubble_black_tea) { Comebuy.new("bubble", "black_tea") }
 
  subject { Comebuy.new("bubble", "milk_tea") }
 
  before do
    @bubble_green_tea = Comebuy.new("bubble", "green_tea")
  end
end
 
RSpec.describe "Quality Examination for bubble_blacktea" do
  include_context "Comebuy"
  it "is black tea" do
    expect(bubble_black_tea.liquid).to eq("black_tea")
  end
end
 
RSpec.describe "Quality Examination for bubble_milk_tea" do
  include_context "Comebuy"
  it "is milk tea" do
    expect(subject.liquid).to eq("milk_tea")
  end
end
 
RSpec.describe "Quality Examination for bubble_green_tea" do
  include_context "Comebuy"
  it "is green tea" do
    expect(@bubble_green_tea.liquid).to eq("green_tea")
  end
end
```

如此一來，便可以在每個 group 裡省略「受測標的」的 code，讓 group 乾淨優雅一點。

# 小結
今天介紹的 shared_context 與昨天的 shared_example 都在做同樣的事情：避免重複。

而避免重複也是 Ruby 強調的重點: Do not repeat yourself.

簡單複習一下：

shared_examples 是將重複的 examples 打包起來抽離出來。

shared_context 是將每個 examples 的受測標的打包並抽離。

明天開始我們將要把 Rspec 帶入 Rails 裡，並逐步介紹好用且常用的測試套件與框架。

