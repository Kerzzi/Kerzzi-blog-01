---
layout: post
title: '「从零开始做购物网站」第13章 增加弹窗登录登出效果'
date: 2017-06-14 23:42
comments: true
categories: 
---


> 大家好，本系列文章主要是对我参加全栈营JDstore魔改大赛作品[「季风 & MONSOON」](http://kerzzi.logdown.com/posts/1903205-magic-change-contest-entries-monsoon)的复盘。由于本人也是新手，文章中一定有很多幼稚的错误或代码，希望大家帮忙指出，可反馈至我的邮箱kerzzi@outlook.com，我会持续完善本系列。非常感谢。
> 本系列是在全栈营JDstore教程基础之上进行魔改，本系列第1-11章节为全栈营JDstore教程，请在完成全栈营JDstore教程的基础上进行测试。

## 目标：
* 增加弹窗登录登出效果

## 步骤：
### Step 0:建立新分支

在终端执行```git checkout -b story07```。

### Step 1:

打开app/views/common/_navbar.html.erb,将代码修改成如下内容
```ruby app/views/common/_navbar.html.erb
<nav class="navbar navbar-default" role="navigation">
    <div class="container-fluid">
        <!-- Brand and toggle get grouped for better mobile display -->
        <div class="navbar-header">
            <a class="navbar-brand" href="/">JDStore </a>
        </div>

        <!-- Collect the nav links, forms, and other content for toggling -->
        <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
            <ul class="nav navbar-nav">
             <li class="active">
               <%= link_to("Products", products_path) %>
             </li>
            </ul>

            <ul class="nav navbar-nav navbar-right">
               <li>
                   <%= link_to carts_path do  %>
                      购物车 <i class="fa fa-shopping-cart"> </i> ( <%= current_cart.products.count %> )
                   <% end %>
               </li>
                <% if !current_user %>
                  <li><a href="#" data-toggle="modal" data-target="#signup-modal">注册</a></li>
                  <li><a href="#" data-toggle="modal" data-target="#login-modal">登入</a></li>
                <% else %>
                  <li class="dropdown">
                    <a href="#" class="dropdown-toggle" data-toggle="dropdown">
                        Hi!, <%= current_user.email %>
                        <b class="caret"></b>
                    </a>
                    <ul class="dropdown-menu">
                         <% if current_user.admin? %>
                           <li>
                              <%= link_to("Admin 选单", admin_products_path ) %>
                           </li>
                           <li>
                              <%= link_to("个人订单列表", account_orders_path ) %>
                           </li>
                         <% end %>
                        <li> <%= link_to(content_tag(:i, '登出', class: 'fa fa-sign-out'), destroy_user_session_path, method: :delete) %> </li>
                    </ul>
                  </li>
                <% end %>
            </ul>
        </div>
        <!-- /.navbar-collapse -->
    </div>
    <!-- /.container-fluid -->
</nav>

<!-- 以下代码实现登入登出的弹窗效果 -->
<div class="modal fade" id="login-modal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true" style="display: none;">
  <div class="modal-dialog" role="document">
    <div class="loginmodal-container">
       <h2>Log in</h2>
       <%= simple_form_for(resource, as: resource_name, url: session_path(resource_name)) do |f| %>
         <div class="form-inputs">
           <%= f.input :email, required: false, autofocus: true %>
           <%= f.input :password, required: false %>
           <%= f.input :remember_me, as: :boolean if devise_mapping.rememberable? %>
         </div>
         <div class="form-actions">
           <%= f.button :submit, "Log in" %>
         </div>
       <% end %>
    </div>
  </div>
</div>

<div class="modal fade" id="signup-modal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true" style="display: none;">
  <div class="modal-dialog">
    <div class="loginmodal-container">
      <h2>Sign up</h2>
        <%= simple_form_for(resource, as: resource_name, url: registration_path(resource_name)) do |f| %>
        <%= f.error_notification %>
        <div class="form-inputs">
          <%= f.input :email, required: true, autofocus: true %>
          <%= f.input :password, required: true, hint: ("#{@minimum_password_length} characters minimum" if @minimum_password_length) %>
          <%= f.input :password_confirmation, required: true %>
        </div>
        <div class="form-actions">
            <%= f.button :submit, "Sign up" %>
        </div>
      <% end %>
    </div>
  </div>
</div>
```

### Step 2:

打开app/helpers/application_helper.rb,将代码修改为如下内容

```ruby app/helpers/application_helper.rb
module ApplicationHelper
  def resource_name
    :user
  end

  def resource
      @resource ||= User.new
  end

  def resource_class
      User
  end

  def devise_mapping
      @devise_mapping ||= Devise.mappings[:user]
  end  
end
```

### Step 3:

打开assets/stylesheets/application.scss，添加如下样式代码

```ruby assets/stylesheets/application.scss

@import url(http://fonts.googleapis.com/css?family=Roboto);

/****** LOGIN MODAL（弹窗登录效果） ******/
.loginmodal-container {
  padding: 30px;
  max-width: 350px;
  width: 100% !important;
  background-color: #F7F7F7;
  margin: 0 auto;
  border-radius: 2px;
  box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
  overflow: hidden;
  font-family: roboto;
}

.loginmodal-container h1 {
  text-align: center;
  font-size: 1.8em;
  font-family: roboto;
}

.loginmodal-container input[type=submit] {
  width: 100%;
  display: block;
  margin-bottom: 10px;
  position: relative;
}

.loginmodal-container input[type=text], input[type=password] {
  height: 44px;
  font-size: 16px;
  width: 100%;
  margin-bottom: 10px;
  -webkit-appearance: none;
  background: #fff;
  border: 1px solid #d9d9d9;
  border-top: 1px solid #c0c0c0;
  /* border-radius: 2px; */
  padding: 0 8px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
}

.loginmodal-container input[type=text]:hover, input[type=password]:hover {
  border: 1px solid #b9b9b9;
  border-top: 1px solid #a0a0a0;
  -moz-box-shadow: inset 0 1px 2px rgba(0,0,0,0.1);
  -webkit-box-shadow: inset 0 1px 2px rgba(0,0,0,0.1);
  box-shadow: inset 0 1px 2px rgba(0,0,0,0.1);
}

.loginmodal {
  text-align: center;
  font-size: 14px;
  font-family: 'Arial', sans-serif;
  font-weight: 700;
  height: 36px;
  padding: 0 8px;
/* border-radius: 3px; */
/* -webkit-user-select: none;
  user-select: none; */
}

.loginmodal-submit {
  /* border: 1px solid #3079ed; */
  border: 0px;
  color: #fff;
  text-shadow: 0 1px rgba(0,0,0,0.1);
  background-color: #4d90fe;
  padding: 17px 0px;
  font-family: roboto;
  font-size: 14px;
  /* background-image: -webkit-gradient(linear, 0 0, 0 100%,   from(#4d90fe), to(#4787ed)); */
}

.loginmodal-submit:hover {
  /* border: 1px solid #2f5bb7; */
  border: 0px;
  text-shadow: 0 1px rgba(0,0,0,0.3);
  background-color: #357ae8;
  /* background-image: -webkit-gradient(linear, 0 0, 0 100%,   from(#4d90fe), to(#357ae8)); */
}

.loginmodal-container a {
  text-decoration: none;
  color: #666;
  font-weight: 400;
  text-align: center;
  display: inline-block;
  opacity: 0.6;
  transition: opacity ease 0.5s;
}

.login-help{
  font-size: 12px;
}
```

### Step 4:

打开app/assets/javascripts/application.js，增加2行代码

```ruby app/assets/javascripts/application.js
   //= require turbolinks
   //= require bootstrap/alert
   //= require bootstrap/dropdown
 + //= require bootstrap-sprockets
 + //= require bootstrap/modal
   //= require_tree .		  
```

### Step 5:将变更 commit 进去 git 里面。

```
git add .
git commit -m "modal login"
git push origin story07
git checkout master
git merge story07
git push origin master
```

## 本章详解
>  努力更新中，上班族请理解。。。。。
 


