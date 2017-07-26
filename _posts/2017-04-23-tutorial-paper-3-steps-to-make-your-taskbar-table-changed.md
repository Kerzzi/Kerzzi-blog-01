---
layout: post
title: '【教程文章】3步让你的工作列表大变样（可根据需求自己定制）'
date: 2017-04-23 12:55
comments: true
categories:
---

昨天下午美化了我的项目展示列表：
前后对比如下：
##### 基于迷你教程的项目列表效果：
![](https://ww2.sinaimg.cn/large/006tNbRwgy1fewiw8npkxj31kw10m76w.jpg)

##### 修改后的效果（**职位推荐**参考了@Jimmy Wang）：
![](https://ww3.sinaimg.cn/large/006tNbRwgy1fewiwxa7rcj31kw2ftdly.jpg)

## 实现教程

### step 0: 原始代码
最初是基于迷你教程制作的，代码如下：
```ruby app/views/jobs/index.heml.erb
<div class="pull-right">
  <%= link_to('<span class="glyphicon glyphicon-gift">创建新项目'.html_safe,
            new_job_path, class: "btn btn-success btn-lg btn3d")%>
</div>

<!-- 上面代码是根据我的网站需要保留的 -->

<table class="table table-boldered table-striped custab" >
  <thead>
      <tr>
          <td>项目名称</td>
          <td>工作城市</td>
          <td>团队名称</td>
          <td>需求专业</td>
          <td>发布时间</td>
      </tr>
  </thead>

  <% @jobs.each do |job| %>
  <tr>

    <td><%= link_to(job.title, job_path(job)) %></td>
    <td><%= job.city %></td>
    <td><%= job.company %></td>
    <td><%= job.category %></td>
    <td><%= job.created_at %></td>
  </tr>
  <% end %>
</table>

```

相关阴影边框效果的样式代码

```ruby  app/assets/stylesheets/application.scss
.custab{
  border: 1px solid #ccc;
  padding: 5px;
  margin: 5% 0;
  box-shadow: 3px 3px 2px #ccc;
  transition: 0.5s;
}

.custab:hover{
  box-shadow: 3px 3px 0px transparent;
  transition: 0.5s;
}

```

### Step1：HTML文件修改，主要说明见注释内容。

代码块：
```ruby app/views/jobs/index.heml.erb
<div class="pull-right">
  <%= link_to('<span class="glyphicon glyphicon-gift">创建新项目'.html_safe,
            new_job_path, class: "btn btn-success btn-lg btn3d")%>
</div>
<p></p>

<div class="text-center">
  <h1><strong>项目列表</strong></h1>
</div>

<!-- 以下为修改内容，上面两块代码是根据我的网站需要保留的。 -->

<div class="row ">
    <!-- 项目列表 -->
    <div class="col-md-9 job-content">
        <% @jobs.each do |job| %>
          <div class=" job-box">
            <!-- 有职位资料时 -->
            <% if @jobs.length > 0 %>
                <div >
                    <h4><strong><%= link_to(job.title, job_path(job)) %></strong></h4>
                </div>
                <div >
                    <p style="width: 800px; overflow: hidden;white-space: nowrap;text-overflow: ellipsis;"><%= job.description %></p>
                    <p><%= link_to( "Read more", job_path(job)) %></p>
                </div>
                <div>
                    <p></p>
                    <p>
                      <sapn class="fa fa-street-view"></sapn> <%= job.city %>|
                      <sapn class="fa fa-calendar"></sapn> <%= job.created_at.strftime("%Y-%m-%d") %>|
                      <sapn class="fa fa-weixin"></sapn> <%= job.posts.count %>|

                      <sapn class="fa fa-tags">Tags : <span class="label label-info"><%= job.company %></span>
                      <a href="#"><span class="label label-success"><%= job.category %></span></a>
                    </p>
                </div>

            <% else %>
            <!-- 没有职位资料 -->
                <div >
                    <h2>此分类暂时没有招聘职位哦，请尝试其他选项。</h2>
                </div>
            <% end %>  <!--结束判断 -->
          </div> <!--结束@jobs循环-->
        <% end %>
    </div>

    <!-- 职位推荐 -->
    <div class="col-md-3 side-box">
      <div class="row">
          <div class="col-md-12 suggest-title">
            <h3><sapn class="fa fa-tags"></sapn> <strong>职位推荐</strong></h3>
          </div>
          <div>
              <% @suggests.each do |job| %>
                  <div class="col-xs-10 col-xs-offset-1 suggest-content">
                      <div>
                        <h4>
                          <%= link_to job_path(job), target: "_blank" do %>
                              <%= job.title %> [<%= job.city %>]
                          <% end %>
                        </h4>
                      </div>
                      <div>
                        <p>招募职位：<%= job.category %></p>
                      </div>
                      <div>
                        <p>团队名称：<%= job.company %></p>
                      </div>
                  </div>
              <% end %> <!-- 结束@suggests循环 -->
          </div>
      </div>
    </div>
</div>


<!-- 分页区块 -->

  <div class="text-center">
    <%= will_paginate @jobs %>
  </div>
```

几点说明：
1. 参考[8-2 （解答）招聘网站第五部分](https://fullstack.xinshengdaxue.com/posts/563) step1，使用```<div class="row"><div class="col-md-9"><div class="col-md-3">```将页面布局分为9：3。一块展示项目列表，一块展示推荐职位。
2.  为部分<div>添加 ```job-content、job-box、side-box、suggest-title、suggest-content```等class类别，以便后续进行格式修改。
3. 如果你想要自己定制效果，只需要将代码放在```<% @jobs.each do |job| %>```和```<% end %>```中间即可。
4. 职位推荐区块，参考了[@Jimmy Wang](https://fullstack.xinshengdaxue.com/works/288)的代码。
5. 这些图标<sapn class="fa fa-street-view"></sapn>，可以在[这个网站](http://fontawesome.io/icons/)找，换成你喜欢的。
6.  这段代码```<p style="width: 800px; overflow: hidden;white-space: nowrap;text-overflow: ellipsis;"><%= job.description %></p>``` 是为了设置说明文字的长度，超过这个长度以省略号显示。亦可以用```<p><%= (truncate((job.description), :length => 100, :omission => '...')).html_safe %></p>```这个代码完成省略显示的效果，我测试了下，有点不同哦，您可以亲自试试。
7. 这段代码```<%= job.created_at.strftime("%Y-%m-%d") %>```是为了将创建日期，转换为年月日显示。

### step2:实现推荐功能
1. 在job的model中建立scope

```ruby app/models/job.rb
  # Scope #
     scope :recent, -> { order("created_at DESC")}
     scope :published, -> { where(:is_hidden => false)}
  +  scope :random5, -> { limit(5).order("RANDOM()") }
```

2. 在job的controller的index action中加入实例变量@suggests

```ruby app/controllers/jobs_controller.rb
  def index

    # 随机推荐五个职位 #
+   @suggests = Job.published.random5
    @jobs = Job.published.recent.paginate(:page => params[:page], :per_page => 10)
  end
```


### step3:加上相应的样式

在SCSS文件中加上相应的样式，请根据需要自行修改。

```ruby  app/assets/stylesheets/application.scss
/*--- Job ---*/

.job-content{
  min-height: 725px;
  margin-top: 100px;

  a{
    text-decoration: none;
  }
}

.side-box{
 display: inline-block;
 position: absolute;
 margin-top: 112px; //这些根据你的页面显示自己调节数值
 margin-left: 35px;
 padding-bottom: 12.5px;
 min-width: 260px;

 background-color: #1A6561;
 color: #fff;
 box-shadow: 2px 2px 3px #aaa;
}

.job-box{
 border: 1.5px solid #ddd;
 box-shadow: 1px 1px 5px #ccc;
 margin-top: 12.5px;
 padding: 15px 1px 1px 20px;
 background-color: #fff;
 color: #555;
 font-size: 16px;

 div{
 margin-bottom: 5px;
 }
}

  .suggest-title{
    padding: 12.5px;
    text-align: center;
    font-size: 20px;
    letter-spacing: 0.5px;
  }


  .suggest-content{
    background-color: #fff;
    padding: 7.5px;
    border: 0.5px dashed #eee;


    h3{
      margin-top: 1px;
      font-size: 16px;
    }

    p{
      font-size: 16px;
      color: #FF7D0A;
      letter-spacing: -1px;
    }

    span{
      color: #aaa;
      font-size: 14px;
      letter-spacing: 0.5px;
    }
  }

```

如果本篇分享对你有帮忙，请帮我的作品：[扬帆社区](https://fullstack.xinshengdaxue.com/works/361) 评论测试下。不需要投票。感觉我这个页面好冷清，都没人去看，哈哈。

**用户故事：**
微博系统，用户间可以互相关注，发布微博（可带图片），朋友圈以动态流展示微博。
用户可以发布项目，其他用户可以申请加入项目、对项目发表评论。
项目管理员可以编辑、删除自己创建的项目。
网站管理员可以删除用户、隐藏或公开项目。

#### 测试账号：

密码均为123456

**网站管理员账号**
*  admin@kerzzi.com

**普通账号：**

*  test1@kerzzi.com
*  test2@kerzzi.com
*  test3@kerzzi.com
*  test4@kerzzi.com
*  test5@kerzzi.com
*  test6@kerzzi.com
*  test7@kerzzi.com

烦请测试管理员账号时，勿删除以上测试账号，非常感谢！
