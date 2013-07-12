---
layout: post
title: ruby使用正则表达式验证数据格式
categories:
- validate
tags:
- validate
---

ruby使用正则表达式验证数据格式
=====================

###下面是我的数据处理，不是通用的，可更具自己的需求，自行修改


```ruby
def self.pro_contrac_phone(str)
    return  "" if str.nil?
    str.gsub!(" ", "-")
    str.gsub!("86-", "") if str =~ /^86-/
    str = "" unless  str  =~/(^[0-9]{3,4}\-[0-9]{3,8}$)|(^[0-9]{3,8}$)|(^\([0-9]{3,4}\)[0-9]{3,8}$)|(^0{0,1}13[0-9]{9}$)/
    str
  end

  def self.pro_mobile_phone(str)
    return  "" if str.nil?
    str.gsub!("-", "")
    str = $1 if str =~ /.*(\d{11}).*/
    str = "" unless  str  =~ /(\d{11})|^((\d{7,8})|(\d{4}|\d{3})-(\d{7,8})|(\d{4}|\d{3})-(\d{7,8})-(\d{4}|\d{3}|\d{2}|\d{1})|(\d{7,8})-(\d{4}|\d{3}|\d{2}|\d{1}))$/
    str
  end

  def self.pro_fax(str)
    return  "" if str.nil? or str.length < 5
    str.gsub!(/[-|\,|－| ]/, "-")
    str = str[0..-2] if str =~ /-$/  #去掉最后一个-
    str = "" unless  str  =~ /^[+]{0,1}(\d){1,3}[ ]?([-]?((\d)|[ ]){1,12})+$/
    str
  end
```
