---
layout: post
title: 使用rubyzip解压zip文件
categories:
- rubyzip
tags:
- zip
- unzip
- rubyzip
---

##使用rubyzip解压zip文件

* gem install rubyzip

```ruby

require 'rubygems'
require 'zip/zip'

def zip_file(src,des='')
  arr = []
  Zip::ZipFile::open(src) do |zfile|
    zfile.each do |zent|
      fpath = File.join(des, zent.name)
      arr << fpath
      next if fpath.nil? or fpath.empty?
      FileUtils.mkdir_p(File.dirname(fpath))
      zfile.extract(zent, fpath){true}
    end
  end
  return arr
end

src = "F:/UtuntuRailsApp/gop/test.zip"
des = "F:/UtuntuRailsApp/gop/#{Time.now.strftime("%Y-%m-%d-%H-%M-%S")}"
p zip_file(src,des)

```

