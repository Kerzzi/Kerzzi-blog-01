---
layout: post
title: '「从零开始做购物网站」第28章 给购物车增加加减按钮'
date: 2017-06-15 00:07
comments: true
categories: 
---


> 大家好，本系列文章主要是对我参加全栈营JDstore魔改大赛作品[「季风 & MONSOON」](http://kerzzi.logdown.com/posts/1903205-magic-change-contest-entries-monsoon)的复盘。由于本人也是新手，文章中一定有很多幼稚的错误或代码，希望大家帮忙指出，可反馈至我的邮箱kerzzi@outlook.com，我会持续完善本系列。非常感谢。
> 本系列是在全栈营JDstore教程基础之上进行魔改，本系列第1-11章节为全栈营JDstore教程，请在完成全栈营JDstore教程的基础上进行测试。

## 目标
> 给购物车增加加减按钮

## 步骤
### Step 0:建立新分支
执行```git checkout -b story20```。

### Step 1:修改app/controllers/cart_items_controller.rb
打开app/controllers/cart_items_controller.rb
```ruby app/controllers/cart_items_controller.rb
     redirect_to carts_path
    end
  
 +  def add_quantity
 +    @cart_item = current_cart.cart_items.find_by_product_id(params[:id])
 +    if @cart_item.quantity < @cart_item.product.quantity
 +         @cart_item.quantity += 1
 +         @cart_item.save
 +         redirect_to carts_path
 +    elsif @cart_item.quantity == @cart_item.product.quantity
 +         redirect_to carts_path, alert: "库存不足！"
 +    end
 +  end
 +
 +  def remove_quantity
 +    @cart_item = current_cart.cart_items.find_by_product_id(params[:id])
 +    if @cart_item.quantity > 0
 +         @cart_item.quantity -= 1
 +         @cart_item.save
 +         redirect_to carts_path
 +    elsif @cart_item.quantity == 0
 +         redirect_to carts_path, alert: "商品不能少于零！"
 +    end
 +  end
 +
    private
```

### Step 2:修改app/views/carts/index.html.erb
打开 app/views/carts/index.html.erb
```ruby app/views/carts/index.html.erb
                         <%= cart_item.product.price %>
                      </td>
                      <td>
 -                        <%= form_for cart_item, url: cart_item_path(cart_item.product_id) do |f| %>
 -                        <%= f.select :quantity, 1..cart_item.product.quantity %>
 -                        <%= f.submit "更新", data: { disable_with: "Submiting..." } %>
 -                        <% end %>
 +                        <div class="count-input">
 +                          <%= link_to("-", remove_quantity_cart_item_path(cart_item.product_id), class: "incr-btn", method: :post) %>
 +                          <input class="quantity" type="text" value="<%= cart_item.quantity %>">
 +                          <%= link_to("+", add_quantity_cart_item_path(cart_item.product_id), class: "incr-btn", method: :post) %>
 +                        </div>
                      </td>
                      <td>
                          <%= link_to cart_item_path(cart_item.product_id), method: :delete do %>
```

### Step 3:增加相应的路由
打开config/routes.rb
```ruby config/routes.rb
 -  resources :cart_items
 +  resources :cart_items do
 +    member do
 +      post :add_quantity
 +      post :remove_quantity
 +    end
 +  end
```

### Step 4:将变更 commit 进去 git 里面。

```
git add .
git commit -m "给购物车增加加减按钮"
git push origin story20
git checkout master
git merge story20
git push origin master
```

## 本章详解
>  努力更新中，上班族请理解。。。。。




