---
layout: post
title: 使用rvm安装及管理ruby
categories:
- rvm
tags:
- rvm
---

使用rvm安装及管理ruby
=====================

###安装curl
1. 查看curl是否已经安装，dpkg -s curl
2. 安装curl，sudo apt-get install curl 
###安装rvm
1. 使用官方推荐的安装方法, curl -L get.rvm.io | bash -s stable
2. 配置.bash_profile, 以便在开启一个终端回话的时候加载RVM
    [[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" # This loads RVM into a shell session. 
3. 查看rvm时候安装成功, rvm -v
4. 查看可安装的ruby版本 rvm list known
5. 安装ruby， rvm install 1.8.7
###配置代理服务器
1. 在执行rvm install 1.8.7时，等待了好长时间没有反应，因为没有配置代理服务器
2. 创建文件~/.curlrc，内容：
    proxy=http://210.101.131.232:8080
3. 重新执行rvm install 1.8.7
4. ok!
5. [代理服务器地址](http://www.veryhuo.com/res/ip/)