---
layout: 
title: 封裝：public / private / protected
date: 2022-08-14 16:57:52
tags:
---
# 封裝的用意

存取控制的作用。

讓某部分方法透過包裝，限制外部的取用，不讓外部使用者或程式直接取用或呼叫。

藉此希望使用者只能夠使用某些功能、某些方法。

# 封裝的方法

共有三種，分別為：public, private, protected. 

# 這三種方法的說明如下：

## public: 

完全公開的方法，內外皆可存取。

```ruby
class Company
  def finance  #public狀態的方法
  end
end
```

---

## private: 

對外限制，不能有特定的receiver，故僅能在類別方法裡使用，且不能有 receiver。

```ruby
class Company
  def finance
    money #成功
    self.money #失敗
  end

  private    #對外限制的私人方法
    def money
    end
end

Mike = Company.new
Mike.finance
Mike.money #失敗
money #失敗
Mike.send (:money) #成功  #在Mike物件上呼叫send公開方法，對Mike send money訊息
而private以下的方法皆會變成private狀態，直到遇見public。
class Company
  private
  def money
  end

  def documents
  end

  public
  def staff
  end
end
```

所以一般大眾寫法是先將public的方法寫在前面，後面再使用private設定其他成私人的方法。

```ruby
class Company
  def staff
  end
  def marketing
  end

  private
  def money
  end
  def documents
  end
end
```

---

## protected : 

對外有限制，不能有receiver，但在class裡面則可有receiver也可以沒有receiver。

```ruby
class Company
  def finance
  money #成功
  self.money #成功
  end
  
  protected
  def money
  end 
end

Mike = Company.new
Mike.finance
Mike.money #失敗
```

通常protected不太會使用，如果會使用到protected，則代表其實是想使用private。

