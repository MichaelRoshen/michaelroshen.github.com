---
layout: post
title: 动态二级下拉框联动
categories:
- JS
tags:
- 二级下拉框
---

###动态二级下拉框联动

###视图层,省份默认从数据库中读取，显示在下拉框中，当选择省份后，添加js，查询数据库，市区联动刷新

```ruby
    地区：省份：
    <%= f.select :location_parent_id_eq, options_for_select(Location.where("parent_id is null").map{|locat| [locat.name, locat.area_code]}, params[:q].present? ? params[:q][:location_parent_id_eq] : nil), {:prompt => "--选择省份--"} %>
    选择市区：<%= f.select :location_area_code_eq, options_for_select([""]), {:prompt => "--选择市区--"}  %>
```

###给省份的下拉框添加事件，下拉框内容变化时，发送get_city_location的请求，获取该省的所有地区
###然后填充到市区的下拉框中

```ruby
//按地区过滤
    $("#q_location_parent_id_eq").change(function(){
      $.get("<%= get_city_location_index_path %>", {parent_area_code:this.value}, function (data){
        var datas = eval(data);
        $("#q_location_area_code_eq option").remove();
        $("<option value=''>"+"--选择市区--"+"</option>").appendTo($("#q_location_area_code_eq"));
        $.each(datas,function(i){
          $("<option value='"+datas[i].area_code+"'>"+datas[i].name+"</option>").appendTo($("#q_location_area_code_eq"));
        })
      });
    });
```

###路由的配置
###注意：发送的请求，get_city_location_index_path
###对应的路由为

```ruby
    get_city_location_index GET    /location/get_city(.:format)    location#get_city
```

```ruby
  resources :location, :only => ["get_city"] do
    collection do
      get :get_city
    end
  end
```

###模型层中，添加get_city方法,返回json数据，传过来的参数-省份

```ruby
#encoding: utf-8
class LocationController < ApplicationController
 
  def get_city
    city = Location.where("parent_area_code = ?",params[:parent_area_code]).where("name <> '县'")
    render :json => city.to_json
  end

end

```
