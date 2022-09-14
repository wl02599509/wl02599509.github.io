---
title: IT 邦鐵人賽 Day 14 - RSpec 語法 context method
date: 2022-09-14 10:19:36
category: iThome
tags: 
- iThome
- Test Automation
- Ruby
- Rspec
---
今天我們要來說的東西比前面幾個方法還簡單很多：context，主要是用來整理 examples 的敘述，讓 readability 直線上升！

# 在一般的情況下

一個 example 由 it 建構，it 後面可以加上敘述文字：

<!--more-->

```ruby
class Member
  attr_accessor :rank
  def initialize(rank)
    @rank = rank
  end
 
  def bonus
    if @rank === 'silver'
      0.9
    elsif @rank === 'golden'
      0.8
    end
  end
end
 
RSpec.describe ".bonus method" do
  let(:member) { Member.new('normal') }
 
  it 'should feedback 10% discount when member is silver rank.' do
    member.rank = 'silver'
    expect(member.bonus).to eq(0.9)
  end
 
  it 'should feedback 20% discount when member is golden rank.' do
    member.rank = 'golden'
    expect(member.bonus).to eq(0.8)
  end
end
```

結果：

```ruby
.bonus method
  should feedback 10% discount when member is silver rank.
  should feedback 20% discount when member is golden rank.
 
Finished in 0.00202 seconds (files took 0.05494 seconds to load)
2 examples, 0 failures
```

不知道各位讀者看完有什麼感覺？那個敘述似乎又臭又長，雖稱不上凌亂，但總覺得不是很優雅，所以我們可以用 context 來整理，將重複的部分抽出來：

# Context

```ruby
RSpec.describe ".bonus method" do
  let(:member) { Member.new('normal') }
 
  context "when member is silver rank" do
    it 'should feedback 10% discount.' do
      member.rank = 'silver'
      expect(member.bonus).to eq(0.9)
    end
  end
 
  context "when member is golden rank" do
    it 'should feedback 20% discount.' do
      member.rank = 'golden'
      expect(member.bonus).to eq(0.8)
    end
  end
end
```

結果：

```ruby
.bonus method
  when member is silver rank
    should feedback 10% discount.
  when member is golden rank
    should feedback 20% discount.
 
Finished in 0.00194 seconds (files took 0.05754 seconds to load)
2 examples, 0 failures
```

如此一來，是不是變得很有結構，閱讀起來也容易很多！

在使用 context 時，要特別注意，慣例上都是以 when, with, without 作為開頭。

另外，context 也可以由 describe 代替：

```ruby
RSpec.describe ".bonus method" do
  let(:member) { Member.new('normal') }
 
  describe "when member is silver rank" do
    it 'should feedback 10% discount.' do
      member.rank = 'silver'
      expect(member.bonus).to eq(0.9)
    end
  end
 
  describe "when member is golden rank" do
    it 'should feedback 20% discount.' do
      member.rank = 'golden'
      expect(member.bonus).to eq(0.8)
    end
  end
end
```

這樣也是一樣的，這就是在前面帶測試時，用的 describe，不知道大家記不記得？

# 小結
我自己比較喜歡使用 context。如果今天有一百個 examples，那這樣加上 RSpec.describe 就有 101 個 describe，我還不多吃三顆葉綠素！使用 context 比較能有點辨識度。

今天的東西輕輕鬆鬆，明天接續有點困難的語法～
