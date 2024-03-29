---
layout: post
title: '「从零开始做购物网站」第21章 增加商品点赞功能'
date: 2017-06-14 23:55
comments: true
categories: 
---


> 大家好，本系列文章主要是对我参加全栈营JDstore魔改大赛作品[「季风 & MONSOON」](http://kerzzi.logdown.com/posts/1903205-magic-change-contest-entries-monsoon)的复盘。由于本人也是新手，文章中一定有很多幼稚的错误或代码，希望大家帮忙指出，可反馈至我的邮箱kerzzi@outlook.com，我会持续完善本系列。非常感谢。
> 本系列是在全栈营JDstore教程基础之上进行魔改，本系列第1-11章节为全栈营JDstore教程，请在完成全栈营JDstore教程的基础上进行测试。


## 目标
> 增加商品点赞功能

## 步骤
### Step 0:建立新分支
执行```git checkout -b story15```。

### Step 1:
打开Gemfile
```ruby Gemfile
  gem 'will_paginate'
  gem 'paperclip'
+ gem 'acts_as_votable', '~> 0.10.0'
```

执行```bundle install```。

### Step 3:建立migration
运行```rails g acts_as_votable:migration```。

运行```rake db:migrate```。


### Step 2:修改product.rb
打开app/models/product.rb，增加一行代码
```ruby app/models/product.rb
  has_one :main_product_image, -> { order(weight: 'desc') },class_name: :ProductImage

+ acts_as_votable

  before_create :set_default_attrs #产品创建之前生成唯一uuid
```

### Step 3:修改routes.rb
在routes.rb中的加入代码
```ruby config/routes.rb
  resources :products do
    member do
      post :add_to_cart
      put "like", to: "products#upvote"
    end
  end
```

### Step 4:修改controller

在app/controllers/products_controller.rb中加入
```ruby app/controllers/products_controller.rb
  before_action :authenticate_user!, only: [:upvote, :downvote]
  ...
  ...
  ...
  def upvote
    @product = Product.find(params[:id])
    @product.upvote_by current_user
    redirect_to :back
  end
  
  def downvote
    @product=Product.find(params[:id])
    @product.downvote_by current_user
    redirect_to :back
  end
```


### Step 5:修改html.erb文件,在需要添加点赞功能的页面加入如下的内容：

在需要添加点赞功能的页面加入如下的内容：
```
<%= link_to like_product_path(product), method: :put do %>
    <div class="pi-thumbs-up">
        赞（<%= product.get_upvotes.size %>）
    </div>
<% end %>
```

在需要添加点差评功能的页面加入如下的内容：
```
<%= link_to dislike_product_path(product), method: :put do %>
<div class="pi-thumbs-down">
    踩（<%=product.get_downvotes.size%>）
</div>
```
其中路径的参数是选择@product还是product要根据具体的页面来确定。

我们这里暂时增加点赞功能，所以

打开app/views/products/show.html.erb，增加如下代码
```ruby app/views/products/show.html.erb
      <h3><strong>现价: ¥<%= @product.price %></strong> </h3>
      <p>购买: <input type="text" name="quantity" value="1" />件</p>

+     <%= link_to like_product_path(@product), method: :put do %>
+         <div class="pi-thumbs-up">
+             赞（<%= @product.get_upvotes.size %>）
+         </div>
+     <% end %>
```


### Step 6:将变更 commit 进去 git 里面。
```
git add .
git commit -m "完成商品点赞功能"
git push origin story15
git checkout master
git merge story15
git push origin master
```

## 本章详解
>  努力更新中，上班族请理解。。。。。

