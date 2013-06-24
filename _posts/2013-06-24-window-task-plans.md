---
layout: post
title: 用ruby创建windows定时任务
categories:
- schtasks
tags:
- schtasks
- jekyll
- blog
---

用ruby创建windows定时任务
=====================

1. windows的定时任务可以在任务计划中进行设置，定时执行批处理文件，可以在批处理文件中
   做一些处理，例如删除过期文件，创建文件夹，调用ruby文件等。
  
2. 创建定时任务：

{ % highlight ruby %}

 def utf82gbk(s)
  require 'iconv'
  Iconv.conv('gbk', 'utf-8', s)
 end

  def create_schtasks_cmd(task_detail)
    cmd_schtasks = "schtasks /create /ru GRANDSOFT\\gbqat /rp 123abc!@#"
    cmd_schtasks += " /sc once"
    cmd_schtasks += " /sd #{task_detail[:start_time].strftime("%Y/%m/%d")}"
    cmd_schtasks += " /st #{task_detail[:start_time].strftime("%H:%M:%S")}"
    cmd_schtasks += " /tn #{utf82gbk(task_detail[:task_name])}"
    cmd_schtasks += " /tr #{utf82gbk(task_detail[:run_file_path])}"
    puts cmd_schtasks
    begin
      require 'rubygems'
      gem 'win32-process', '~> 0.6.0'
      require 'win32/process'
      Process.create(:command_line => cmd_schtasks)
    rescue => e
      p e
    end
  end

{ % endhighlight %}

3. 删除定时任务：

{ % highlight ruby %}

 def run_cmd(cmd, retry_times = 0, retry_delay = 2)
    puts cmd
    out = `#{cmd}`
    puts out
    unless $? == 0
      if retry_times == 0
        fail(cmd + ' run failure: ' + out)
      else
        sleep(retry_delay)
        out = run_cmd(cmd, retry_times - 1, 1.618 * retry_delay)
      end
    end
    out
  end
  
  run_cmd("schtasks /delete /tn #{task_detail[:task_name]} /f")
  
  { % endhighlight %}
  
  4. 读取定时任务：
  
  { % highlight ruby %}
  
    def read_schtasks
    connent = run_cmd("schtasks /query /fo table /nh")
    tasks_name = []
    connent.split("\n").each do |item|
      next if item.empty?
      tasks_name << item.split(/\s/)[0]
    end
    tasks_name
  end
  
    { % endhighlight %}


5. [更多Schtasks命令](http://technet.microsoft.com/zh-cn/library/cc772785.aspx)