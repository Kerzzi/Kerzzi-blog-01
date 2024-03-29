---
layout: post
title: '「从零开始做购物网站」第32章 美化服务页面'
date: 2017-06-15 00:10
comments: true
categories: 
---


> 大家好，本系列文章主要是对我参加全栈营JDstore魔改大赛作品[「季风 & MONSOON」](http://kerzzi.logdown.com/posts/1903205-magic-change-contest-entries-monsoon)的复盘。由于本人也是新手，文章中一定有很多幼稚的错误或代码，希望大家帮忙指出，可反馈至我的邮箱kerzzi@outlook.com，我会持续完善本系列。非常感谢。
> 本系列是在全栈营JDstore教程基础之上进行魔改，本系列第1-11章节为全栈营JDstore教程，请在完成全栈营JDstore教程的基础上进行测试。


## 目标
> 美化服务页面

## 步骤
### Step 0:建立新分支
执行```git checkout -b story24```。

### Step 1:修改app/views/static_pages/help.html.erb
打开app/views/static_pages/help.html.erb，修改成如下代码
```ruby app/views/static_pages/help.html.erb
<!-- 中间区域介绍内容 -->
  <div class="row">
    <div class="landing-page-mid">
      <div class="container">
        <div class="row">
          <div class="col-md-12">
            <h3 class="tagline text-center">就像李荣浩的那首不将就</h1>
              <br/>
              <p class="col-md-6 col-md-offset-3 text-center tagp">

                改变生活也从不将就开始，开始选择质感·颜值,
                  如果带点有趣那就完美了</p>
          </div>
        </div>
      </div>
    </div>
  </div>

<!-- question -->
  <div id="question" class="row">
    <div class="module-name decorative">
      <div class="text-center">
        <h2>FAQ</h2>
      </div>
    </div>
    <div class="col-md-10 col-md-offset-1">
      <div class="col-md-3 col-sm-6">
      <p class="sub-question"><b>Q：</b>购买了seeyoo.的商品什么时候发货呢？</p>
      <p class="answer"><b>A：</b>我们统一采用顺丰快递配送，而且近期为了感谢新老客户的支持，我们专请意大利设计师为商品设计包装礼盒，在我们商城购买5次以上可享受此项服务</p>
      </div>
      <div class="col-md-3 col-sm-6">
      <p class="sub-question"><b>Q：</b>对于商品的质量，你们有把关吗？</p>
      <p class="answer"><b>A：</b>由于有些网站为了牟利是会让不良商家入驻，但是我们在入驻前都会有严格的审查机制，以保证商品的质量，保证消费者的权益是我们的核心任务</p>
      </div>
      <div class="col-md-3 col-sm-6">
        <p class="sub-question"><b>Q：</b> 如果购买后不是很喜欢可以退货吗？</p>
        <p class="answer"><b>A：</b>可以的，我们有相关规定在购买后15天内包退，30天内如有质量问题可找商家包换，所以买了不喜欢也不怕哦</p>
      </div>
      <div class="col-md-3 col-sm-6">
        <p class="sub-question"><b>Q：</b>新客户有什么优惠吗？</p>
        <p class="answer"><b>A：</b>有的，新客户第一次购买商品时可以免费领取我们的6折优惠券，所以有喜欢的尽快趁机购买哦</p>
      </div>
    </div>
  </div>
```

###Step 2:修改app/assets/stylesheets/application.scss
打开app/assets/stylesheets/application.scss
```ruby app/assets/stylesheets/application.scss
/****** 服务页面样式 ******/
 // 中间区域介绍内容样式
  .landing-page-mid {                                   //给图片整好形
    height: 534px;
    background-size: cover;                             //确保图片完全撑开
    margin-top: -15px;
    background-position: center center;
    background-repeat: no-repeat;
    background-image: url(http://ww4.sinaimg.cn/large/006tNbRwgy1ffzzmaueitj31kw18s7wk.jpg);
  }
  .landing-page-mid .tagline {                          //编写标题样式
    text-align: center;
    color: #7f8c8d;
    letter-spacing: 0.1em;                              //把字间距撑开，视觉感更优雅
    font-size: 1.6em;
    margin-top: 3.2em;
  }
  .landing-page-mid .tagp {                             //编写内容样式
    text-align: center;
    color: #666;
    font-size: 1.2em;
    margin-top: 2.5em;
    line-height: 2.5em;                                 //行间距也要拉开大一些
    letter-spacing: 0.18em;                             //把字间距撑开，视觉感更优雅
  }

/* question */

#question {
  background-color: #EFEBE9;
  margin-bottom: 45px;
  margin-top:-20px;
  color: none;
  font-family: 300;

  .col-md-3 {
    padding: 30px;
    border-right: 10px solid #aaa;
    height: 250px;
  }
  .col-md-3:last-child {
    border: none;
  }
  .sub-question {
    margin-bottom: 10px;
    //color: rgba(0, 0, 0, 0.4);
  }
  .answer {
    //color: rgba(0, 0, 0, 0.4);
  }
}
/****** 完成服务页面样式 ******/
```

### Step 3:将变更 commit 进去 git 里面。

```
git add .
git commit -m "美化服务页面"
git push origin story24
git checkout master
git merge story24
git push origin master
```

## 本章详解
>  努力更新中，上班族请理解。。。。。

