---
layout: post
title: 递归获取文件夹下所有文件
categories:
- 递归

递归获取文件夹下所有文件
=====================

###下面是一个递归获取指定文件夹下所有xlsx文件的例子

```ruby
class FilePro 
  def self.get_file_list(path,arr =[])
    Dir.entries(path).each do |sub|
      next if  sub == '.' or  sub == '..'
      if File.directory?("#{path}/#{sub}")
        get_file_list("#{path}/#{sub}",arr)
      elsif sub =~ /.*.xlsx/
        arr <<  "#{path}/#{sub}"
      end
    end
    arr
  end
end
```
