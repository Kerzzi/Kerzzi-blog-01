---
layout: post
title: '「从零开始做购物网站」第36章 简单美化后台管理各个页面'
date: 2017-06-16 23:40
comments: true
categories: 
---

> 大家好，本系列文章主要是对我参加全栈营JDstore魔改大赛作品[「季风 & MONSOON」](http://kerzzi.logdown.com/posts/1903205-magic-change-contest-entries-monsoon)的复盘。由于本人也是新手，文章中一定有很多幼稚的错误或代码，希望大家帮忙指出，可反馈至我的邮箱kerzzi@outlook.com，我会持续完善本系列。非常感谢。
> 本系列是在全栈营JDstore教程基础之上进行魔改，本系列第1-11章节为全栈营JDstore教程，请在完成全栈营JDstore教程的基础上进行测试。

## 目标
*  美化后台管理各个页面

## 步骤
### Step 0:
执行```git checkout -b story28```。

### Step 1: 美化后台管理首页：后台说明
打开app/views/admin/sessions/new.html.erb，完整代码如下：
```ruby app/views/admin/sessions/new.html.erb
<div class="panel panel-info">
  <!-- Default panel contents -->
  <div class="panel-heading" align="center"><h1>后台管理指南</h1></div>

  <div class="panel-body">
    <div align="center">
      <img width="980px" src="https://ww4.sinaimg.cn/large/006tKfTcgy1fg1kcuh6o9j318g0p0qbf.jpg">
    </div>
    <hr/ width="25%">
    <div style="margin:0px 65px 0px 65px;">
      <p>管理员大人，吉祥！</p>
      <p>我是简洁清爽的后台~\(≧▽≦)/~啦啦啦！</p>
      <p>这是一份后台管理指南，有3点要求需要您遵守。</p>
      <p>第1条：严格遵守后台管理指南！</p>
      <p>第2条：请严格遵守第1条要求！</p>
      <p>第3条：请参照第2条执行！</p>
    </div>
  </div>

  <!-- List group -->
  <ul class="list-group" style="margin:0px 65px 0px 65px;">
    <li class="list-group-item" >分类管理：商品建立之前必须建立商品分类，目前支持2级分类！</li>
    <li class="list-group-item">商品管理：商品必须至少上传5张图片！我们喜欢高清无码大图，你懂得！</li>
    <li class="list-group-item">订单管理：这个很简单的~\(≧▽≦)/~啦啦啦！</li>
    <li class="list-group-item">竞拍管理：OMG，这可是我们的主打功能，可恶的开发小哥到现在还没做出来！</li>
    <li class="list-group-item">返回前台：哎呦！不要去找前台嘛！</li>
    <li class="list-group-item">退出：后台管理完成记得退出哦！</li>
  </ul>
</div>
```

### Step 2: 美化后台商品管理页面
打开app/views/admin/products/index.html.erb，完整代码如下：
```ruby app/views/admin/products/index.html.erb
<div class="panel panel-info">
  <!-- Default panel contents -->
  <div class="panel-heading" align="center"><h1>商品管理</h1></div>

  <div class="panel-body">
    <div class="pull-right">
      <%= link_to "新建商品", new_admin_product_path, class: "btn btn-info" %>
    </div>

    <div style="margin-left: 50px;">
      <p>管理员，您好！</p>
      <p>目前共有商品  <strong><%= @products.total_entries %></strong>  种！</p>
    </div>
    <hr/ width="25%">
    <div class="row" >
      <table class="table table-striped">
        <tr>
          <th>ID</th>
          <th>商品名称</th>
          <th>商品UUID</th>
          <th>原价</th>
          <th>现价</th>
          <th>库存</th>
          <th>上架状态</th>
          <th>商品管理</th>
        </tr>

        <% @products.each do |product| %>
          <tr>
            <td><%= product.id %></td>
            <td><%= product.title %></td>
            <td><%= product.uuid %></td>
            <td><%= product.msrp %></td>
            <td><%= product.price %></td>
            <td><%= product.quantity %></td>
            <td><%= product.status %></td>
            <td>
              <%= link_to "编辑", edit_admin_product_path(product) %>
              <%= link_to "删除", admin_product_path(product), method: :delete, 'data-confirm': "确认删除吗?" %>
              <%= link_to "图片管理", admin_product_product_images_path(product_id: product) %>
            </td>
          </tr>
        <% end %>
      </table>
      <div class="page-box" align="center">
        <%= will_paginate @products %>
      </div>
    </div>
  </div>
</div>
```

打开app/views/admin/products/new.html.erb，完整代码如下：
```ruby app/views/admin/products/new.html.erb
<div class="panel panel-info">
  <!-- Default panel contents -->
  <div class="panel-heading" align="center">
    <h1><%= @product.new_record? ? "新建商品" : "修改商品 ##{params[:id]}" %></h1>
  </div>
  <div class="panel-body" style="margin-left: 270px;">
    <div class="form-body">
      <%= form_for @product,
        url: (@product.new_record? ? admin_products_path : admin_product_path(@product)),
        method: (@product.new_record? ? 'post' : 'put'),
        html: { class: 'form-horizontal' } do |f| %>

        <% unless @product.errors.blank? %>
          <div class="alert alert-danger">
            <ul class="list-unstyled">
              <% @product.errors.messages.values.flatten.each do |error| %>
                <li><i class="fa fa-exclamation-circle"></i> <%= error %></li>
              <% end -%>
            </ul>
          </div>
        <% end %>


        <div class="form-group">
          <label for="title" class="col-sm-2 control-label">名称:*</label>
          <div class="col-sm-5">
            <%= f.text_field :title, class: "form-control" %>
          </div>
        </div>
        <div class="form-group">
          <label for="category_id" class="col-sm-2 control-label">所属分类:</label>
          <div class="col-sm-5">
            <select name="product[category_id]">
              <option></option>
              <% @root_categories.each do |category| %>
                <optgroup label="<%= category.title %>"></optgroup>

                <% category.children.each do |sub_category| %>
                  <option value="<%= sub_category.id %>" <%= @product.category_id == sub_category.id ? 'selected' : '' %>><%= sub_category.title %></option>
                <% end -%>
              <% end -%>
            </select>   请选择2级分类
          </div>
        </div>
        <div class="form-group">
          <label for="title" class="col-sm-2 control-label">上下架状态:*</label>
          <div class="col-sm-5">
            <select name="product[status]">
              <% [[Product::Status::On, '上架'], [Product::Status::Off, '下架']].each do |row| %>
                <option value="<%= row.first %>" <%= 'selected' if @product.status == row.first %>><%= row.last %></option>
              <% end %>
            </select> 若商品图片少于5张请勿上架！
          </div>
        </div>
        <div class="form-group">
          <label for="quantity" class="col-sm-2 control-label">库存*:</label>
          <div class="col-sm-5">
            <%= f.text_field :quantity, class: "form-control" %> 必须为整数
          </div>
        </div>
        <div class="form-group">
          <label for="price" class="col-sm-2 control-label">价格*:</label>
          <div class="col-sm-5">
            <%= f.text_field :price, class: "form-control" %>必须为数字
          </div>
        </div>
        <div class="form-group">
          <label for="msrp" class="col-sm-2 control-label">原价*:</label>
          <div class="col-sm-5">
            <%= f.text_field :msrp, class: "form-control" %>必须为数字
          </div>
        </div>
        <div class="form-group">
          <label for="description" class="col-sm-2 control-label">描述*:</label>
          <div class="col-sm-5">
            <%= f.text_area :description, class: "form-control" %>
          </div>
        </div>
        <div class="form-group">
          <div class="col-sm-offset-2 col-sm-8">
            <%= f.submit (@product.new_record? ? "新建商品" : "编辑商品"), class: "btn btn-default" %>
          </div>
        </div>
      <% end %>
    </div>
  </div>
</div>
```

### Step 3: 美化后台商品图片管理页面
打开app/views/admin/product_images/index.html.erb，完整代码如下：
```ruby app/views/admin/product_images/index.html.erb
  <div class="panel panel-info">
    <!-- Default panel contents -->
    <div class="panel-heading" align="center"><h1>商品 #<%= @product.id %> <%= @product.title %></h1>
    </div>
    <div class="panel-body">
      <div class="pull-right">
        <%= form_tag admin_product_product_images_path(product_id: @product), method: :post, enctype: "multipart/form-data", class: "form-horizontal" do %>
          <input type="file" name="images[]" multiple class="image-input" />
          <%= submit_tag "上传", class: "btn btn-primary" %>
        <% end %>
      </div>
      <div style="margin-left: 50px;">
        <h4>请至少上传5张照片！商品图片少于5张严禁上架！</h4>
        <h4>图片权重最高者，为主图片！</h4>
      </div>
    </div>
    <hr/ width="25%">

    <!-- Table -->
    <table class="table table-striped ">
      <tr>
        <th>ID</th>
        <th>商品图片</th>
        <th>图片权重</th>
        <th>图片操作</th>
      </tr>

      <% @product_images.each do |product_image| %>
        <tr>
          <td><%= product_image.id %></td>
          <td><img width="500px" src=<%= qiniu_image_path(product_image.image.url) %>></td>
          <td>
            <%= form_tag admin_product_product_image_path(@product, product_image), method: :put do %>
              <input type="text" value="<%= product_image.weight %>" name="weight" />
              <%= submit_tag "更新", class: "btn btn-default btn-xs" %>
            <% end -%>
          </td>
          <td>
            <%= link_to "删除", admin_product_product_image_path(@product, product_image), method: :delete, 'data-confirm': "确认删除吗?" %>
          </td>
        </tr>
      <% end %>
    </table>
  </div>
```

### Step 4:美化后台订单管理页面
打开app/views/admin/orders/index.html.erb，完整代码如下：
```ruby app/views/admin/orders/index.html.erb
<div class="panel panel-info">
  <!-- Default panel contents -->
  <div class="panel-heading" align="center"><h1>订单管理 </h1></div>
  <div class="panel-body" style="margin-left: 50px;">
    <p>管理员可以取消订单或设为发货。</p>
  </div>
  <hr/ >
  <h2 align="center">订单列表 </h2>
  <hr/ width="25%">

  <!-- Table -->
  <table class="table  table-striped">
    <thead>
      <tr>
        <th>ID</th>
        <th>生成时间</th>
        <th>订购者</th>
        <th>订单状态</th>
      </tr>
    </thead>
    <tbody>
      <% @orders.each do |order| %>
      <tr>
        <td> <%= link_to(order.id, admin_order_path(order) ) %> </td>
        <td> <%= order.created_at.to_s(:long) %> </td>
        <td> <%= order.user.email %></td>
        <td> <%= order.aasm_state %> </td>
      </tr>
      <% end %>

    </tbody>
  </table>

</div>
```

打开app/views/admin/orders/show.html.erb,将第一行代码进行修改
```ruby app/views/admin/orders/show.html.erb
-   <div class="row">
+   <div class="container">
```

### Step 5: 优化后台管理：商品类别页面

打开app/views/admin/categories/index.html.erb，完整代码如下：
```ruby app/views/admin/categories/index.html.erb
<div class="panel panel-info">
  <!-- Default panel contents -->
  <div class="panel-heading" align="center"><h1>商品类别管理</h1></div>

  <div class="panel-body">
    <div class="pull-right">
      <%= link_to "新建分类", new_admin_category_path, class: "btn btn-info" %>
    </div>

    <div style="margin-left: 50px;">
      <p>管理员，您好！</p>
      <% if @category %>
        当前一级分类为: <strong><%= @category.title %></strong> ，该分类下共有 <strong><%= @categories.total_entries %></strong> 个二级分类。
      <% else %>
        当前共计 <strong><%= @categories.total_entries %></strong> 个一级分类!
      <% end %>
    </div>
    <hr/ width="25%">
    <div class="row" >
      <table class="table  table-striped">
        <tr >
          <th>类别 ID</th>
          <th>分类名称</th>
          <th>权重</th>
          <th >分类管理</th>
        </tr>

        <% @categories.each do |category| %>
          <tr >
            <td><%= category.id %></td>
            <td><%= category.title %></td>
            <td><%= category.weight %></td>
            <td >
              <% if category.root? %>
                <%= link_to("查看子分类", admin_categories_path(id: category)) %>
              <% else %>
                <%= link_to("返回上级分类", admin_categories_path) %>
              <% end %>
              <%= link_to("编辑", edit_admin_category_path(category)) %>
              <%= link_to("删除", admin_category_path(category), :method => :delete,:data => { :confirm => "Are you sure?" }) %>
            </td>
          </tr>
        <% end %>
      </table>

      <div class="page-box" align="center">
      <%= will_paginate @categories %>
      </div>
    </div>
  </div>
</div>
```


打开app/views/admin/categories/new.html.erb，完整代码如下：
```ruby app/views/admin/categories/new.html.erb
<div class="panel panel-info" >
  <!-- Default panel contents -->
  <div class="panel-heading" align="center">
    <h1><%= @category.new_record? ? "新建分类" : "修改分类 ##{params[:id]}" %></h1>
  </div>
  <div class="panel-body">
    <div class="form-body" style="margin-left: 270px;">
      <%= form_for @category,
        url: (@category.new_record? ? admin_categories_path : admin_category_path(@category)),
        method: (@category.new_record? ? 'post' : 'put'),
        html: { class: 'form-horizontal' } do |f| %>

        <% unless @category.errors.blank? %>
          <div class="alert alert-danger">
            <ul class="list-unstyled">
              <% @category.errors.messages.values.flatten.each do |error| %>
                <li><i class="fa fa-exclamation-circle"></i> <%= error %></li>
              <% end -%>
            </ul>
          </div>
        <% end -%>

        <div class="form-group">
          <label for="ancestry" class="col-sm-2 control-label">所属分类:</label>
          <div class="col-sm-5">
            <select name="category[ancestry]">
              <option></option>
              <% @root_categories.each do |category| %>
                <% next if category == @category %>
                <option value="<%= category.id %>" <%= @category.ancestry == category.id.to_s ? 'selected' : '' %>><%= category.title %></option>
              <% end -%>
            </select>
            为空为一级分类
          </div>
        </div>
        <div class="form-group">
          <label for="title" class="col-sm-2 control-label">名称:*</label>
          <div class="col-sm-5">
            <%= f.text_field :title, class: "form-control" %>
          </div>
        </div>
        <div class="form-group">
          <label for="weight" class="col-sm-2 control-label">权重:</label>
          <div class="col-sm-5">
            <%= f.text_field :weight, class: "form-control" %> 数值越大越靠前
          </div>
        </div>
        <div class="form-group">
          <div class="col-sm-offset-2 col-sm-8">
            <%= f.submit (@category.new_record? ? "新建分类" : "编辑分类"), class: "btn btn-default" %>
          </div>
        </div>
      <% end %>
    </div>
  </div>

</div>
```

### Step 6:将变更 commit 进去 git 里面。

```
git add .
git commit -m "简单美化后台管理各个页面"
git push origin story28
git checkout master
git merge story28
git push origin master
```

## 本章详解
>  努力更新中，上班族请理解。。。。。


