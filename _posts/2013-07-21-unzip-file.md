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

* 有些linux版本中unzip命令不支持 -O 选项，则可以使用rubyzip,文件名含有中文的时候，解压出来的是乱码，使用Iconv进行转换后，即可

```ruby

def get_file_list(path,arr =[])
    Dir.entries(path).each do |sub|
      next if  sub == '.' or  sub == '..'
      if File.directory?("#{path}/#{sub}")
        get_file_list("#{path}/#{sub}",arr)
      elsif sub =~ /.*.xlsx/ or sub =~ /.*.xls/
        arr <<  "#{path}/#{sub}"
      end
    end
    arr
  end

  def unzip(src,des='')
  arr = []
  Zip::ZipFile::open(src) do |zfile|
    zfile.each do |zent|
       name = Iconv.conv('utf-8' , 'gbk', zent.name)
      fpath = File.join(des, name)
      arr << fpath
      next if fpath.nil? or fpath.empty?
      FileUtils.mkdir_p(File.dirname(fpath))
      zfile.extract(zent, fpath){true}
    end
  end
  return arr
end

  def zip_file(filename)
    des = Rails.root.join("tmp",filename.gsub(".zip", "")).to_s
    filepath = Rails.root.join("tmp", filename).to_s
    FileUtils.makedirs(des)
    unzip(filepath, des)
    xlsxs = get_file_list(des)
    return xlsxs
  end

  def upload_file(file,extname,target_dir)
    if file.nil? || file.original_filename.empty?
      return false, "空文件或文件名称错误"
    else
      timenow = Time.now
      filename = file.original_filename
      file_load_name = timenow.strftime("%d%H%M%S") + filename
      if extname.include?(File.extname(filename).downcase)
        path = Rails.root.join(target_dir)
        FileUtils.makedirs(path)
        File.open(File.join(path, file_load_name ) , 'wb') do
          |f| f.write(file.read)
        end
        return true, File.join(file_load_name )
      else
        return false, "必须是#{extname}类型文件"
      end
    end
  end
  
```

