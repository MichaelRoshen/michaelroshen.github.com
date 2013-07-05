---
layout: post
title: 使用Roo导入excel到数据库
categories:
- Roo
tags:
- excel
- roo
- zip
---

使用Roo导入excel到数据库
=====================

###安装Roo
使用ruby1.9.3版本，安装roo的时候需要指定版本1.9.5
在Gemfile中添加 gem 'roo', '1.9.5'

```ruby
gem install roo -v '1.9.5'
```

###cannot load such file -- zip/zipfilesystem
启动程序的时候会报这样的错误，在Gemfile中添加
gem 'rubyzip', :require => 'zip/zip'，即可解决

###导入excel
给Product添加import类方法

```ruby
class Product < ActiveRecord::Base
  attr_accessible :name, :price, :released_on

  def self.import(file)
    spreadsheet = Excel.new(file)
    header = spreadsheet.row(1)
    (2..spreadsheet.last_row).each do |i|
      row = Hash[[header, spreadsheet.row(i)].transpose]
      product = find_by_id(row["id"]) || new
      product.attributes = row.to_hash.slice(*accessible_attributes)
      product.save!
    end
  end
end
```

###添加rake任务

```ruby
require File.expand_path('../config/application', __FILE__)

TestDb::Application.load_tasks

desc "import product to db"
task :import => :environment do |t, args|
  Product.import("f:/test.xls")
end  
```
