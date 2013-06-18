---
layout: post
title: 创建github博客记录
categories:
- github
tags:
- github
- jekyll
- blog
---

## 步骤

* 创建github账号
* 创建username.github.com或者username.github.io的Repositroy，名字要和github账号相同
* 在Repositroy Setting中可以自动生成站点，而且有默认的模板，可以根据自己的喜好选择
   Automatic Page Genergator,首次生成需要等待10分钟左右，再访问username.github.io才能生效
   也有可能失败，如果时间过长没有生成，可以联系jekyll客服，另外也可以搭建本地的jekyll
   运行环境，进行测试
   - 安装git
   - 安装jekyll(需要先装Devkit，详见百度)
   $ gem install jekyll
   $ git clong git@github.com:username/username.github.com.git username.github.com
   $ cd username.github.com
   $ jekyll server --watch
* 页面的布局，可根据自己的需要进行修改
  - _layouts 模板存放位置
  - _posts 博文粗放位置
  - _config.yml 配置信息
  - _index.html 首页内容
* _config.yml配置内容
  - markdown: redcarpet       markdown解释引擎
  - pygments: true               代码高亮
  - permalink: /:year/:month/:title   博文访问及存放路径，jekyll会根据设置的模式生成博文存放路径及访问路径
  - paginate: 16                   分页 
