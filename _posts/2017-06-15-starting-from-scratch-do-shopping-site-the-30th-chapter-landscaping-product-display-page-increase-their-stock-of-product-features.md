---
layout: post
title: '「从零开始做购物网站」第30章 美化商品展示页面，增加收藏商品功能'
date: 2017-06-15 00:08
comments: true
categories: 
---


> 大家好，本系列文章主要是对我参加全栈营JDstore魔改大赛作品[「季风 & MONSOON」](http://kerzzi.logdown.com/posts/1903205-magic-change-contest-entries-monsoon)的复盘。由于本人也是新手，文章中一定有很多幼稚的错误或代码，希望大家帮忙指出，可反馈至我的邮箱kerzzi@outlook.com，我会持续完善本系列。非常感谢。
> 本系列是在全栈营JDstore教程基础之上进行魔改，本系列第1-11章节为全栈营JDstore教程，请在完成全栈营JDstore教程的基础上进行测试。


## 目标
> 美化商品展示页面，增加收藏商品功能

## 步骤

### Step 0:建立新分支
在终端执行 ```git checkout -b story22```。

### Step 1: 新增model favorite

在终端执行```rails g model favorite```。

打开app/models/favorite.rb
```ruby app/models/favorite.rb
class Favorite < ApplicationRecord
  belongs_to :user
  belongs_to :product
end
```

打开db/migrate/20170614122223_create_favorites.rb
```ruby db/migrate/20170614122223_create_favorites.rb
class CreateFavorites < ActiveRecord::Migration[5.0]
  def change
    create_table :favorites do |t|
      t.integer :product_id
      t.integer :user_id
      
      t.timestamps
    end
  end
end
```

在终端执行```rails db:migrate```。

### Step 2:修改app/models/product.rb，建立关联

打开app/models/product.rb,修改如下
```ruby app/models/product.rb
 -  has_many :reviews
 +  has_many :reviews, dependent: :destroy
 +  has_many :favorites
 +  has_many :fans, :through => :favorites, :source => :user
  
    acts_as_votable
```

### Step 3:修改app/models/user.rb，建立关联
打开app/models/user.rb,修改如下
```ruby app/models/user.rb
   has_many :orders
 -  has_many :reviews
 +  has_many :reviews, dependent: :destroy
 +  has_many :favorites
 +  has_many :favorite_products, :through => :favorites, :source => :product
 +
 +  def is_fan_of?(group)
 +    favorite_products.include?(group)
 +  end
  
    def admin?
      is_admin
```

### Step 4:新增controller favorites
执行```rails g controller favorites```。

打开app/controllers/favorites_controller.rb，修改成如下内容
```ruby app/controllers/favorites_controller.rb
class FavoritesController < ApplicationController
  def index
    @products = current_user.favorite_products
  end
end
```

打开app/controllers/products_controller.rb
```ruby app/controllers/products_controller.rb
class ProductsController < ApplicationController

  before_action :authenticate_user!, only: [:upvote, :downvote, :favorite, :unfavorite]

  def index
    @products = Product.onshelf.page(params[:page] || 1).per_page(params[:per_page] || 12)
                .order("id desc").includes(:main_product_image)
  end

  def show
   @product = Product.find(params[:id])
  end

  def add_to_cart
    @product = Product.find(params[:id])
    if !current_cart.products.include?(@product)
      current_cart.add_product_to_cart(@product)
      flash[:notice] = "你已成功将 #{@product.title} 加入购物车"
    else
      flash[:warning] = "你的购物车内已有此物品"
    end
    redirect_to :back
  end

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

  def favorite
    @product = Product.find(params[:id])
    current_user.favorite_products << @product
    flash[:notice] = "您已收藏宝贝"
    redirect_to :back
  end

  def unfavorite
    @product = Product.find(params[:id])
    current_user.favorite_products.delete(@product)
    flash[:notice] = "您已取消收藏宝贝"
    redirect_to :back
  end
end

```

### Step 9:增加相应的路由
打开config/routes.rb
```ruby config/routes.rb
+  resources :favorites
  resources :products do
    member do
      post :add_to_cart
+     post :favorite
+     post :unfavorite
      put "like", to: "products#upvote"
      put "dislike", to: "products#downvote"
    end
    resources :reviews
  end
```

### Step 10:修改app/views/products/show.html.erb
打开app/views/products/show.html.erb
```ruby app/views/products/show.html.erb
<div class="show-page">

    <ol class="breadcrumb">
      <li><a href="<%= root_path %>">所有宝贝</a></li>
      <li class="active"><%= @product.category.parent.title %></li>
      <li><a href="<%= category_path @product.category %>"><%= @product.category.title %></a></li>
      <li class="active"><%= @product.title %></li>
    </ol>
  <!-- Start of product card -->
  <div class="container card">
    <!-- Start of left card -->
    <div class="preview col-md-7 " style="">
      <% if @product.product_images.present? %>
        <% if @product.product_images.count > 4 %>
          <div class="preview-pic tab-content" >
            <div class="tab-pane active" id="pic-1"><img src=<%= qiniu_image_path(@product.product_images[0].image.url) %>></div>
            <div class="tab-pane" id="pic-2"><img src=<%= qiniu_image_path(@product.product_images[1].image.url) %>></div>
            <div class="tab-pane" id="pic-3"><img src=<%= qiniu_image_path(@product.product_images[2].image.url) %>></div>
            <div class="tab-pane" id="pic-4"><img src=<%= qiniu_image_path(@product.product_images[3].image.url) %>></div>
            <div class="tab-pane" id="pic-5"><img src=<%= qiniu_image_path(@product.product_images[4].image.url) %>></div>
          </div>
          <ul class="preview-thumbnail nav nav-tabs">
            <li class="active"><a data-target="#pic-1" data-toggle="tab"><img src=<%= qiniu_image_path(@product.product_images[0].image.url) %>></li>
            <li><a data-target="#pic-2" data-toggle="tab"><img src=<%= qiniu_image_path(@product.product_images[1].image.url) %>></li>
            <li><a data-target="#pic-3" data-toggle="tab"><img src=<%= qiniu_image_path(@product.product_images[2].image.url) %>></li>
            <li><a data-target="#pic-4" data-toggle="tab"><img src=<%= qiniu_image_path(@product.product_images[3].image.url) %>></li>
            <li><a data-target="#pic-5" data-toggle="tab"><img src=<%= qiniu_image_path(@product.product_images[4].image.url) %>></li>
          </ul>
        <% else %>
          <div class="preview-pic tab-content" >
            <img src=<%= qiniu_image_path(product.main_product_image.image.url) %>>
          </div>
        <% end %>
      <% else %>
        <div class="preview-pic tab-content" >
          <%= image_tag("http://placehold.it/200x200&text=No Pic") %>
        </div>
      <% end %>
    </div>
    <!-- End of left card -->
    <!-- Start of right cart -->
    <div class="details col-md-5" style="margin-top:50px;">
      <h3 class="product-title"><%= @product.title %></h3>
      <div class="price-service">
        <p class="price">原价 <span class="rmb">￥ </span><span class="msrp"><%= @product.msrp.to_i %></span> 元</p>
        <p class="price">现价 <span class="rmb">￥ </span><span class="price"><%= @product.price %></span> 元</p>
        <p class="service">服务 <span class="service-detail">7天无忧退货 | 48小时快速退款 | 正品保证</span></p>
      </div>
      <div class="quantity-size">
        <p class="delivery">运费 <span class="delivery">全场包邮</span></p>
        <p class="quantity">库存 <span class="quantity"><%= @product.quantity %></span> 件 </p>
        <!-- <p>购买: &nbsp &nbsp &nbsp<input type="text" style="width:50px" name="quantity" value="1" />&nbsp件</p> -->
      </div>
      <div class="action">
        <div class="col-md-10" >
             <% if @product.quantity.present? && @product.quantity > 0 %>
                <%= link_to("加入购物车", add_to_cart_product_path(@product), method: :post,
                            class: "add-to-cart btn btn-block") %>
             <% else %>
               已销售一空，无法购买
             <% end %>
        </div>
        <div class="col-md-2"  >
          <% if current_user %>
           <% if !current_user.is_fan_of?(@product) %>
             <%= link_to favorite_product_path(@product), :class => "like btn btn-default", method: :post do %>
               <i class="fa fa-heart fa-lg"></i>
             <% end %>
           <% else %>
             <%= link_to unfavorite_product_path(@product), :class => "unlike btn btn-default", method: :post do %>
               <i class="fa fa-heart fa-lg"></i>
             <% end %>
           <% end %>
         <% else %>
           <%= link_to favorite_product_path(@product), :class => "like btn btn-default",method: :post do %>
             <i class="fa fa-heart fa-lg"></i>
           <% end %>
         <% end %>
        </div>

      </div>
      <div class="row share">
        <hr />
        <div class="col-md-6">
            <p> 收藏人气（<%= @product.fans.count %>） </p>
        </div>

        <div class="col-md-6">
          <%= link_to like_product_path(@product), method: :put do %>
            <div class="pi-thumbs-up">
              点赞（<%= @product.get_upvotes.size %>）
            </div>
          <% end %>
        </div>
      </div>
      <div class="row share">
        <div class="col-md-6 ">
            <p><i class="fa fa-share-alt" aria-hidden="true"></i> 分享 </p>
        </div>
        <div class="col-md-6" style="margin-left:-40px;">
          <%= social_share_button_tag(@product.title) %><!-- 社交分享 -->
        </div>
      </div>
    </div><!-- End of left card -->

  </div><!-- End of product card -->
  <!-- Start of product description & reviews -->
  <div class="row product_description col-md-12">
    <div class="tabs" >
      <div class="tabbable-panel">
        <div class="tabbable-line">
          <ul class="nav nav-tabs">
            <li role="presentation" class="active">
              <a href="#tab_default_1" data-toggle="tab">宝贝详情</a>
            </li>
            <li>
              <a href="#tab_default_2" data-toggle="tab">宝贝评价(<%= @product.reviews.count %>) </a>
            </li>
          </ul>
          <br />
          <div class="tab-content">
            <div class="tab-pane active" id="tab_default_1">
               <p class="product-description"><%= @product.description.html_safe %></p>
               <p class="product-description2 text-center">产品展示</p>
                <!--商品大图展示 -->
               <div class="row">
                 <% @product.product_images.each do |product_image| %>
                   <div class="col-xs-12 col-md-12 product-show-detail">
                       <img  src=<%= qiniu_image_path(product_image.image.url) %>>
                   </div>
                 <% end %>
               </div>
               <div class="faq">
                 <p class="faq-title text-center">常见问题</p>
                 <p class="question">使用什么快递发货？</p>
                 <p class="answer">伴客默认使用顺丰快递发货，配送范围覆盖全国大部分地区（港澳台地区除外）。</p>
                 <p class="question">如何申请退货？</p>
                 <p class="answer">
                   1.自收到商品之日起7日内，顾客可申请无忧退货，退款将原路返还，不同的银行处理时间不同，预计1-5个工作日到账；<br />
                   2.退货流程：<br />
                   确认收货-申请退货-客服审核通过-用户寄回商品-仓库签收验货-退款审核-退款完成；<br />
                   3.因MONSOON产生的退货，如质量问题，退货邮费由MONSOON承担，退款完成后钱款将沿原渠道返还。因客户个人原因产生的退货，购买和寄回运费由客户个人承担。<br />
                 </p>
                 <p class="question">如何开具发票？</p>
                 <p class="answer">如需开具普通发票，请在下单时联系客服办理，我们将虽货物一起快递给您；</p>
               </div>
            </div><!-- End of tab_default_1 -->

            <div class="tab-pane" id="tab_default_2">
                <div class="row">
                  <div class="col-sm-12 ">
                    <h4>
                      <%= @product.reviews.count %> Reviews
                    </h4>
                    <hr>
                    <div >
                      <%= render @product.reviews%>
                      <%= render "reviews/form"%>
                    </div>
                  </div>
                </div>
              </div><!-- End of tab_default_2 -->
            </div><!-- End of tab-content -->
          </div><!-- End of tabbable-line -->
        </div><!-- End of tabbable-panel -->
      </div><!-- End of tabs -->
    </div><!-- End of product description & reviews -->
</div><!-- End of product show page -->

```

修改app/assets/stylesheets/products.scss，增加相应的样式
```ruby app/assets/stylesheets/products.scss
/* show page */
div.social-share-button {
    margin: 0 auto;
}

div.social-share-button .ssb-icon {
    background-size: 10px 10px;
    height: 10px;
    width: 10px;
    margin-right: 15px;
    border-radius: 50%;
}

a.btn:hover{
  opacity:0.5;
  color:#fff;
}

a.btn{
height:40px;
font-size: 18px;
}


.product-thumbnail{
  border: 1px solid #999;
}
.produst-list{
  height: 200px;
  overflow: hidden;
  img{
    width: 100%;
  }
}


/* Product#show */

.show-page {
  width: 1150px;
  margin: 0 auto;
  // padding-top: 80px;
}

.show-option {
  margin-top: 20px;
  color: #999;
  a {
    color: #999;
  }
  a:hover {
    color: #999;
    text-decoration: none;
  }
}

.show-page .card {
  width: 1150px;
  background: #fff;
  margin-bottom: 20px;
  padding-bottom: 15px;
  border: 1px solid #ddd;
  border-radius: 10px;
}

.show-page .tabs {
  border: 1px solid #ddd;
  border-radius: 10px;
}


/* product show page left card */

.preview {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: vertical;
  -webkit-box-direction: normal;
  -webkit-flex-direction: column;
      -ms-flex-direction: column;
          flex-direction: column;
  margin-top:30px;
}

@media screen and (max-width: 996px) {
  .preview {
      margin-bottom: 20px;
  }
}

.preview-pic {
  -webkit-box-flex: 1;
  -webkit-flex-grow: 1;
      -ms-flex-positive: 1;
          flex-grow: 1;
  height: 500px;
  img{
    width: 100%;
    height: 500px;
    overflow: hidden;
  }
}

.preview-thumbnail.nav-tabs {
  border: none;
  margin-top: 15px; }

.preview-thumbnail.nav-tabs li {
  width: 18%;
  margin-right: 2.5%;
}
.preview-thumbnail.nav-tabs li img {
  max-width: 100%;
  display: block;
  height: 60px;
  border: 1px solid #ddd;
}
.preview-thumbnail.nav-tabs li a {
  padding: 0;
  margin: 0;
}
.preview-thumbnail.nav-tabs li:last-of-type {
  margin-right: 0;
}

.tab-content {
  overflow: hidden;
  padding-top: 15px;
}

/* product show page right card */

.details {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: vertical;
  -webkit-box-direction: normal;
  -webkit-flex-direction: column;
      -ms-flex-direction: column;
          flex-direction: column;
}

.product-title {
  margin-top: 0;
}

.price-service{
  // background-color: rgb(245,245,245);
  margin-top: 30px;
  margin-bottom: 30px;
  padding-left: 10px;
  padding-right: 10px;
  color: #666;
  border-top: 1px dotted #dedede;
  border-bottom: 1px dotted #dedede;
  p.price{
    span.rmb{
      color: #666;
      margin-left: 20px;
    }
    span.price{
      color: #d7282d;
      font-size: 30px;
    }
    span.msrp {
      color: #666;
      font-size: 20px;
      text-decoration: line-through;
    }
  }
  p.service{
    span.service-detail{
      margin-left: 20px;
    }
  }
}
.quantity-size{
  padding-left: 10px;
  padding-right: 10px;
  color: #666;
  p.quantity{
    span.quantity{
      margin-left: 20px;
    }
  }
  p.sizes{
    span.sizes{
      margin-left: 20px;
    }
  }
  p.delivery{
    span.delivery{
      margin-left: 20px;
    }
  }

}

a.btn:hover{
  opacity:0.5;
  color:#fff;
}

a.btn{
height:40px;
font-size: 18px;
}

.action {
  margin-top:30px;
  padding-left:0;padding-right:0px;
  .col-md-10{
    padding-left:0;padding-right:0px;
  }
  .col-md-2{
    padding-left:10px;padding-right:0px;
  }
  a.buy{
    background-color:rgb(245,190,126);color: #fff;margin-bottom:10px;
  }
  a.add-to-cart{
    border-color:rgb(245,190,126);background-color:#fff;color:rgb(245,190,126);
  }
  a.like{
    border-color:rgb(245,190,126);background-color:rgb(245,190,126);
    i{
      color:#fff;
    }
  }
  a.unlike{
    border-color:rgb(245,190,126);background-color:#fff;
    i{
      color:rgb(245,190,126);
    }
  }
}
.favor{
  padding-left: 10px;
  padding-right: 10px;
  color: #666;
}

.share{
  color:#666;
  margin-top:30px;
  hr{
    border-top: 1px dotted #dedede;
    margin-right: 15px;
  }
}
$size: 20px;

div.social-share-button {
  margin: 0;

  .ssb-icon {
    background-size: $size $size;
    height: $size;
    width: $size;
    border-radius: 50%;
        margin-right:0px;
  }
}


/* product show page product description */

.show-page .col-md-8 {
  padding: 0;
  p.product-description{
    padding-top: 30px;
    padding-bottom: 30px;
    color: #666;
    border-bottom: 1px dotted #dedede;
  }
  p.product-description2{
    padding-top: 20px;
    padding-bottom: 20px;
    color: #666;
    font-size: 15px;
  }
  img {
    //width: 100%;
    -webkit-animation-name: opacity;
            animation-name: opacity;
    -webkit-animation-duration: .3s;
            animation-duration: .3s;
  }
  p.faq-title{
    padding-top: 20px;
    padding-bottom: 20px;
    color: #666;
    font-size: 15px;
  }
  p.question{
    color:#666;
  }
  p.answer{
    color: #999;
    margin-bottom: 30px;
  }
  .faq{
    border-top: 1px dotted #dedede;
    margin-top: 20px;
    font-weight: 200;
  }
}

.tab-content #tab_default_1 {

  .product-show-detail{
    border: 1px solid #dedede;
    padding-left: 0px;
    padding-right: 0px;
    img {
      width: 100%;
      -webkit-animation-name: opacity;
              animation-name: opacity;
      -webkit-animation-duration: .3s;
              animation-duration: .3s;
    }
  }

}


.tabs {
  background-color:#fff;
  border: 1px solid #ddd;
  // width: 950px;
  margin-right: -30px;
  margin-bottom: 50px;
  padding-left: 15px;
  padding-right: 15px;
  padding-bottom: 15px;
}



.tabbable-line > .nav-tabs {
  border: none;
  margin: 0px;
}
.tabbable-line > .nav-tabs > li {
  margin-right: 2px;
}
.tabbable-line > .nav-tabs > li > a {
  border: 0;
  margin-right: 0;
  color: #737373;
}
.tabbable-line > .nav-tabs > li > a > i {
  color: #a6a6a6;
}
.tabbable-line > .nav-tabs > li.open, .tabbable-line > .nav-tabs > li:hover {
  border-bottom: 4px solid rgb(245,190,126);
}
.tabbable-line > .nav-tabs > li.open > a, .tabbable-line > .nav-tabs > li:hover > a {
  border: 0;
  background: none !important;
  color: #333333;
}
.tabbable-line > .nav-tabs > li.open > a > i, .tabbable-line > .nav-tabs > li:hover > a > i {
  color: #a6a6a6;
}
.tabbable-line > .nav-tabs > li.open .dropdown-menu, .tabbable-line > .nav-tabs > li:hover .dropdown-menu {
  margin-top: 0px;
}
.tabbable-line > .nav-tabs > li.active {
  border-bottom: 4px solid rgb(245,190,126);
  position: relative;
}
.tabbable-line > .nav-tabs > li.active:hover {
  border:none;
}
.tabbable-line > .nav-tabs > li.active > a {
  border: 0;
  color: #333333;
}
.tabbable-line > .nav-tabs > li.active > a > i {
  color: #404040;
}
.tabbable-line > .tab-content {
  margin-top: -3px;
  background-color: #fff;
  border: 0;
  border-top: 1px solid #eee;
  padding: 15px 0;
}
.tabbable-line > .nav-tabs>li.active>a, .tabbable-line > .nav-tabs>li.active>a:focus, .tabbable-line > .nav-tabs>li.active>a:hover {
border: 0;
border-bottom: 0px solid rgb(245,190,126);
}

/* product show page product reviews */

form{
  margin-top: 15px;
  margin-bottom: 15px;
}

ul.list-group{
  margin-bottom: 5px;
}

.round-image-50 {
  background-color: white;
  border: 1px solid #d9d9d9;
  border-radius: 25px;
  -moz-border-radius: 25px;
  -webkit-border-radius: 25px;
  height: 50px;
  width: 50px;
  overflow: hidden;
  text-align: center;
  img { width: 100% }
}

/* product show page proposal products */

.proposal-product {
  border: 1px solid #ddd;
  background: #fff;
}
```

## Step 11: 将变更 commit 进去 git 里面。

```
git add .
git commit -m "美化商品展示页面，增加收藏商品功能"
git push origin story22
git checkout master
git merge story22
git push origin master
```


## 本章详解
>  努力更新中，上班族请理解。。。。。

