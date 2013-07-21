---
layout: post
title: linux解压zip文件
categories:
- rubyzip
tags:
- rubyzip
- unzip
---

## linux解压zip文件

* 使用rubyzip进行文件解压的时候，在window上可以正常执行，但是如果是在window中压缩好的文件上传到linux上，再进行
* 解压，则会因为文件名含有中文，导致解压失败
* 一种解决方法是使用linux命令unzip进行解压, 下面是一个解压zip文件，获取所有xlsx文件的例子

```ruby
 def get_file_list(path,arr =[])
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

  def zip_file(filename)
    des = Rails.root.join("tmp",filename.gsub(".zip", "")).to_s
    filepath = Rails.root.join("tmp", filename).to_s
    `unzip -O CP936 #{filepath} -d #{des}`
    xlsxs = get_file_list(des)
    return xlsxs
  end
```

