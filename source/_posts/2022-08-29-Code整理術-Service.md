---
title: Code 整理術: Service
date: 2022-08-29 11:34:45
category: Ruby
tags: Ruby
---
# 前言
當我們在寫專案時，有很多的功能會寫在各自的 MVC 裡。

但當這項功能是 CRUD 以外的功能，我們究竟要把它寫在哪呢？

# Service

通常這類功能，像是金流串接，需要將參數加密後發 request 到金流廠商，接著再接收金流廠商發過來的 request 並解碼，再回傳相關資料。

這樣的 code 寫在哪比較合理？

看起來 model 是一個選項，但你要選擇哪個 model? 新增一個 model 來做這件事情似乎也有點奇怪。

所以我們可以把這類的功能整理到 app/service/payment 裡。

也就是在 app 底下新增 service 與 payment 資料夾，並在底下建立 xxxx.rb，並在這建立一個 PORO 底下則開始寫功能的 code: 
(以下為串接綠界金流的 code )
```ruby
module Payment
  class EcpayRequest
    require 'uri'
    require 'openssl'
    require 'CGI'
  
    def initialize(merchant_trade_date, 
                   merchant_trade_no, 
                   title, 
                   pirce)
  
      @params = {
        "TradeDesc": "專案贊助交易", 
        "PaymentType": "aio", 
        "MerchantTradeDate": merchant_trade_date, 
        "MerchantTradeNo": merchant_trade_no, 
        "MerchantID": 3002607, 
        "ReturnURL": "https://f25a-103-3-192-33.jp.ngrok.io/payment/returnpage",
        "ItemName": title 
        "TotalAmount": price, 
        "ChoosePayment": "ALL", 
        "IgnorePayment": "WebATM#ATM#CVS#BARCODE",
        "EncryptType": 1
      }
    end
  
    def perform(params)
      encrypt(params)
    end
  
    private
  
    def encrypt(params) 
      # 1.由A到Z的順序並轉換為 query string
      order_query_string = URI.encode_www_form(params.to_a.sort!)
      # 2.前後加 Key 跟 IV
      key_vi = "HashKey=pwFHCqoQZGmho4w6&" + order_query_string + "&HashIV=EkRm7iFT261dpevs"
      # 3.進行 URL encode 並轉小寫
      url_encode = (CGI.escape key_vi).downcase.gsub!(/%25/, '%').gsub!(/%2b/, '+')
      # 4.sha256加密轉大寫
      encrypted_result = Digest::SHA256.hexdigest(url_encode).upcase
    end
  
    def request
      #尚未
    end
  end
end
```

這時就可以使用類別來做事：
```ruby
Payment::EcpayRequest
```

沒意外，這時會顯示 找不到常數 Payment::EcpayRequest 的錯誤。

# 解決辦法

會出現該訊息是因為，Rails 抓不到這個 class，所以要把 config/application 大概第 21 行程式碼打開：
```ruby
config.eager_load_paths << Rails.root.join("extras")
```
這樣 Rails 在 load 的時候，就會抓到了。