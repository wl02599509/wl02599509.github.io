---
title: IT邦鐵人賽Day18-Factory_bot幫你輕鬆測（一）
date: 2022-09-19 00:24:02
category: iThome
tags: 
- iThome
- Test Automation
- Ruby on Rails
- Rspec
- Factory_bot
---
昨天已經在 Rails  裡安裝了 rails-rspec，但通常不太可能只單靠 rails-rspec 完成測試的撰寫。

# WHY

我想，如果非 Rails 初學者或至少有學過 CRUD，就會知道 Rails 有大量的關聯性，一對一、一對多、多對多。

<!--more-->

彼此關聯的相依會影響各自的存在條件。

## 假如 User has_many projects / Projects belongs_to user：

如果要 Projects.create，那它必須要依附在 User，才有辦法 create。

```ruby
RSpec.describe Project, type: :model do
  it "should not be created" do
    p1 = Project.create("title": "123")
    expect(p1.persisted?).to eq false
  end
 ```

在執行測試時，也是一樣會受到關聯性的影響。

所以這時可能會在測試，把 project 需要的 user 也建立出來：

```ruby
  it "should be true" do
    u1 = User.create
    p1 = u1.projects.create
    expect(p1.persisted?).to eq true
  end
end
```

## 但萬一今天的關聯一大串呢？ 
like belongs_to message, message belongs_to project, project belongs_to user
 
如果用上面的方式來寫的話：

```ruby
it "should be true" do
   u1 = User.create
   p1 = u1.projects.create
   m1 = p1.messages.create
   l1 = m1.like.create
   expect(l1.persisted?).to eq true
 end
 ```

為了要測 like，結果前面要把所有的關聯都建立起來，一大串的前置作業，真的太惱人了，也沒有人會這樣寫。

所以這個麻煩的關聯問題，就可以用 Factory_bot 來解決！（當然，Factory_bot 不只這樣！）

# Factory_bot

如同他的名字，工廠機器，這個套件可以幫我們像工廠一樣，製造出 model 的物件，既然是製造 model 的物件，那也一定會有上述關聯問題，所以交給 factory_bot 就能一次搞定，而且還不需要打一大堆程式碼。

## Gem factory_bot_rails

```ruby
group :development, :test do
 gem 'factory_bot_rails', '~> 6.2'
end
```

放好後一樣執行 bundle install。

完成後，只要使用指令 rails g，就會在 spec/factories 裡產生一樣名稱的 _rspec.rb 檔，跟 rspec_rails 一樣。

如果沒有，那可以手動在自己建資料夾。

接著需要在 spec_helper.rb 裡，將以下程式碼貼在 RSpec.configure 的 block 裡：

```ruby
 config.include FactoryBot::Syntax::Methods
```

這是為了讓 Rails 有 factory 的 method。

如此一來，在執行 rspec 時，讀取 rails_helper.rb 就會抓到 factory_bot_rails 的這個設定檔，就會有 method 了。
如果按照官網，另外把設定 code 寫在 factories.rb 的方式，會導致在終端機上執行 rails 的指令失敗。

## 進入工廠

簡單來說，factory_bot_rails 就是一個工廠，能幫我們在 rspec-rails 裡，依照工廠的模具，製造出需要的資料。

所以在 factories 裡的 _spec.rb 裡，放的程式碼就代表著工廠模具：

```ruby
FactoryBot.define do
  factory :user do
    name { "Mike" }
    age { 28 }
    height {183}
  end
end
```

其實可讀性蠻高的，從第一行開始往下解讀：

這座工廠的定義是要生產 user，那生產 user 需要的材料有 name, age, height 欄位，內容則是 Mike, 28, 183。

User 類別的工廠，會產出含有 name, age, height 各屬性與其值的實體變數。

如此一來，這座工廠就定位了，我們就可以在 rspec 讓工廠生產 project。

## 製造方法

在 rspec 裡，我們可以使用 build(:model_name) 與 create(:model_name) 來取代冗長的 Model.new(“name”: “Mike”, “age”: 28, “height”: 183) 程式碼。 

```ruby
RSpec.describe User, type: :model do
 it "has factory" do
   u1 = create(:user)
   expect(u1.name).to eq 'Mike'
 end
end
```

因為工廠已經都把模具準備好了，我們在 rspec 裡，只需要按下開關 build 或 create 就好。

## 建立關聯

最重要的功能，就是幫我們輕鬆把關聯帶進模具裡，產出來的每個實體變數都是已經關聯好的：

```ruby
FactoryBot.define do
  factory :project do
    user
    title { "Ruby on Rails" }
  end
end
```

沒錯，在 factory :project 的 block 裡加上這座工廠 belongs_to 的對象 (project belongs_to user)，這樣就完成啦！

但他原型是長這樣：

```ruby
FactoryBot.define do
 factory :project do
   association :user, factory :user
   title { "Ruby on Rails" }
 end
end
```

意思就是，project 的關聯是 user，而 user 在 user 的工廠裡。

# 小結

今天主要是介紹基本的工廠使用方式，明天我們來針對工廠製造的細節去探究清楚。
