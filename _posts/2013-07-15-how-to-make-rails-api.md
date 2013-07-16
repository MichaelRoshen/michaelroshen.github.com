---
layout: post
title: 如何做api
categories:
- api
tags:
- faraday
---

##学习做自己的api

* 使用faraday_middleware插件，Faraday, 它是一个ruby的http客户端，用类似rack middleware的方式来定制各种http调用
* 简单的说就是发送一个http请求给我们的web程序，然后调用相应的api，进行数据处理后，返回给我们结果

* 在gemfile中加入faraday_middleware
* 在config/initializers/下创建文件, init_faraday.rb,  文件内容如下：

```ruby
UPLOAD_URL = "http://127.0.0.1:3000"
$conn = Faraday.new(:url => UPLOAD_URL) do |faraday|
  faraday.request  :url_encoded
  faraday.adapter  Faraday.default_adapter
end
```

* 以上定义了，我们要请求的地址，发送请求的字符编码，适配器，默认为NetHttp
* 在我们的控制器中就可以发送请求了

```ruby
  def self.send_http_request(params)
      begin
        response = $conn.post "/api/dps/prosessor", params
        results = JSON.parse(response.body)
      rescue
        return results
      end
  end
```

* 实际地址为：http://127.0.0.1:3000/api/dps/prosessor + params
* 下一步只需要添加相应的控制器就可以了
* 在文件夹app/controller/api/dps_controller.rb下

```ruby
class Api::DpsController < ActionController::Base
  
  def prosessor
     p "---------------"
     p params
     p "---------------"
     render :json => {}
  end
  
end
```

对应的api routes配置如下：

```ruby
  namespace :api do
    resources :dps do
      collection do
        post :prosessor
  end
    end
  end
```


