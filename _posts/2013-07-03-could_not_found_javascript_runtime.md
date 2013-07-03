---
layout: post
title: 解决Ubuntu下找不到Javascript Runtime问题
categories:
- Javascript Runtime
tags:
- nodejs
- execjs
- therubyracer
---

解决Ubuntu下找不到Javascript Runtime问题
=====================

###Rails3.2.8 Could not find a JavaScript runtime.错误的解决方法

```ruby
sudo apt-get install python-software-properties  
sudo add-apt-repository ppa:chris-lea/node.js  
sudo apt-get update  
sudo apt-get install nodejs  
```