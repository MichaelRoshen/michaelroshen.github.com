---
layout: post
title: Rails Routes
categories:
- Routes
tags:
- rails
- routes
---


###rails routes的几种写法

1. 默认生成7种方法：

```ruby
resources :groups
```

                       groups GET    /groups(.:format)                          groups#index
                              POST   /groups(.:format)                          groups#create
                    new_group GET    /groups/new(.:format)                      groups#new
                   edit_group GET    /groups/:id/edit(.:format)                 groups#edit
                        group GET    /groups/:id(.:format)                      groups#show
                              PUT    /groups/:id(.:format)                      groups#update
                              DELETE /groups/:id(.:format)                      groups#destroy

2. 在此基础上进行扩展collection,  格式： 资源名/方法名

```ruby

  resources :base_material_types do
    collection do
      get :sub_types
    end
  end


sub_types_base_material_types GET    /base_material_types/sub_types(.:format)   base_material_types#sub_types
          base_material_types GET    /base_material_types(.:format)             base_material_types#index
                              POST   /base_material_types(.:format)             base_material_types#create
       new_base_material_type GET    /base_material_types/new(.:format)         base_material_types#new
      edit_base_material_type GET    /base_material_types/:id/edit(.:format)    base_material_types#edit
           base_material_type GET    /base_material_types/:id(.:format)         base_material_types#show
                              PUT    /base_material_types/:id(.:format)         base_material_types#update
                              DELETE /base_material_types/:id(.:format)         base_material_types#destroy
```

3. member ，格式，资源名/id/方法名

```ruby

  resources :companies do
    collection do
      post :set_reviewed_state
      post :set_location
    end
    member do
      get :set_delete_status
    end
  end

 set_reviewed_state_companies POST   /companies/set_reviewed_state(.:format)    companies#set_reviewed_state
       set_location_companies POST   /companies/set_location(.:format)          companies#set_location
    set_delete_status_company GET    /companies/:id/set_delete_status(.:format) companies#set_delete_status
                    companies GET    /companies(.:format)                       companies#index
                              POST   /companies(.:format)                       companies#create
                  new_company GET    /companies/new(.:format)                   companies#new
                 edit_company GET    /companies/:id/edit(.:format)              companies#edit
                      company GET    /companies/:id(.:format)                   companies#show
                              PUT    /companies/:id(.:format)                   companies#update
                              DELETE /companies/:id(.:format)                   companies#destroy
```

4. 注意如果资源写成了单数，注意生成的path和url不同，使用only则不再生成默认的7中方法，只生成only中定义的方法

```ruby

  resources :location, :only => ["get_city"] do
    collection do
      get :get_city
    end
  end

  location：(单数)
  get_city_locations GET    /locations/get_city(.:format)              locations#get_city
  locations:(复数)
  get_city_location_index GET    /location/get_city(.:format)               location#get_city
```
