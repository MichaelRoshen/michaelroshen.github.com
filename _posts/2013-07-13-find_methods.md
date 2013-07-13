---
layout: post
title: rails查询方法
categories:
- find
tags:
- find method
---

rails查询方法
=============

###find_by_batches一次查询指定数量的数据，返回的是对象数组

```ruby
Company.find_in_batches(:batch_size => 100, :conditions => "reviewed_state != 's'") {|coms| p coms.size }
```
>>

```ruby
 Company Load (10.5ms)  SELECT "companies".* FROM "companies" WHERE (reviewed_state != 's') ORDER BY "companies"."id" ASC LIMIT 100
100
  Company Load (8.2ms)  SELECT "companies".* FROM "companies" WHERE (reviewed_state != 's') AND ("companies"."id" > 2143) ORDER BY "companies"."id" ASC LIMIT 100
100
  Company Load (6.2ms)  SELECT "companies".* FROM "companies" WHERE (reviewed_state != 's') AND ("companies"."id" > 2243) ORDER BY "companies"."id" ASC LIMIT 100
100
  Company Load (4.4ms)  SELECT "companies".* FROM "companies" WHERE (reviewed_state != 's') AND ("companies"."id" > 2343) ORDER BY "companies"."id" ASC LIMIT 100
44
 => nil 
```

###find_each,返回的是对象

```ruby
Company.find_each(:batch_size => 5, :start => 350) {|v| p v.name}
```

[查看更多](http://wuhuizhong.iteye.com/blog/1046218)
