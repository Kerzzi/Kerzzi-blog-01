---
layout: post
title: '「从零开始做购物网站」第35章 美化关于页面'
date: 2017-06-16 23:40
comments: true
categories: 
---

> 大家好，本系列文章主要是对我参加全栈营JDstore魔改大赛作品[「季风 & MONSOON」](http://kerzzi.logdown.com/posts/1903205-magic-change-contest-entries-monsoon)的复盘。由于本人也是新手，文章中一定有很多幼稚的错误或代码，希望大家帮忙指出，可反馈至我的邮箱kerzzi@outlook.com，我会持续完善本系列。非常感谢。
> 本系列是在全栈营JDstore教程基础之上进行魔改，本系列第1-11章节为全栈营JDstore教程，请在完成全栈营JDstore教程的基础上进行测试。

## 目标
*  美化关于页面
*  效果如下
![](https://ww1.sinaimg.cn/large/006tNc79gy1fgru4ebagjj31kw1k8gpn.jpg)

## 步骤
### Step 0:
执行```git checkout -b story27```。

### Step 1: 修改app/views/static_pages/about.html.erb页面
打开app/views/static_pages/about.html.erb，将代码替换为如下内容：
``` ruby app/views/static_pages/about.html.erb
<div class="container">

  <!-- 网站简介 -->
  <div class="container about-title-box">
    <span class="about-title-e">季   风 。</span><br>
    <h1 class="about-title-c">ABOUT US</h1>
  </div>

  <p class="about-info">
    「季风 & MONSOON」 是一个全球性网上商城 <br>
    主要售卖有趣的生活用品 <br><br>
    这里有你可能没见过的黑科技小物件 <br>
    脑洞大开的设计品 <br>
    当然也包含D&W这类精美的物件 <br>
    在这些商品中<br>你能真切感受到科技和艺术真的已经完美的融入生活<br>
    发现最美好物，生活从不将就！「季风」为了你的有趣生活！<br>

  </p>

  <hr>

  <!-- 团队介绍 -->
  <div class="row">
    <div class="col-md-12 about-info about-img ">
      <h3 class="tagline" align="center" >团队介绍</h3>
      <hr width="10%">
      <div class="col-xs-12 col-sm-6 col-md-6">
        <div class="col-xs-4">
          <%= image_tag("http://ww1.sinaimg.cn/large/006tNbRwly1fg1w3pa0fqj30pm0t0js9.jpg", class: "img-circle img-responsive img-center frontpage-img") %>
        </div>
        <div class="col-xs-8">
          <h5><a href="http://lll96-blog.logdown.com/" target="_blank">LLL96（胡颖）</a></h5>
          <p><strong>微信：ying96z</trong></p>
          <p>
            编程路上新手。<br>
            喜欢咖啡和泡各种茶、健身爱好者。 <br>
            喜欢好玩的新鲜事。<br>
            欣赏江湖义气、话痨爱交朋友。<br>
          </p>
        </div>
      </div>
      <div class="col-xs-12 col-sm-6 col-md-6">
        <div class="col-xs-4">
          <%= image_tag("http://ww1.sinaimg.cn/large/006tNbRwly1fg1w0lp12kj30mi0mjgmd.jpg", class: "img-circle img-responsive img-center frontpage-img") %>
        </div>
        <div class="col-xs-8">
          <h5><a href="http://kerzzi.logdown.com/" target="_blank">柯子(刘铮)</a></h5>
          <p><strong>微信：zhulanwa</strong></p>
          <p>
            爱好编程的辐射防护工程师。 <br>
            喜欢读书、思考。爱游戏、爱生活。<br>
            专注个人成长、努力活在未来。<br>
            欢迎和我一起探索这个世界！<br>
          </p>
        </div>
      </div>
    </div>
  </div>

</div>

```

### Step 2: 增加相应的样式
打开app/assets/stylesheets/static_pages.scss，增加相应的样式：
```ruby app/assets/stylesheets/static_pages.scss
/* 关于我们页面样式 */

// 标题样式
.about-title-box {
  margin-top: 5px;
  margin-bottom: 0px;
  background: #CFD8DC;

}
.about-title-e {
  font-weight: lighter;
  font-size: 2.3em;
}
.about-title-c {
  font-weight: lighter;
  font-size: 2.5em;
  letter-spacing: 0.16em;
  margin: 2px auto 50px;
}

// 信息样式
.about-info {
  font-size: 1.1em;
  letter-spacing: 0.2em;
  font-weight:normal;
  line-height: 2em;
}
.about-img {
  height: 20em;
  margin-top: 50px;
  margin-bottom: 0px;
}
```

### Step 3:将变更 commit 进去 git 里面。

```
git add .
git commit -m "美化关于页面"
git push origin story27
git checkout master
git merge story27
git push origin master
```

## 本章详解
>  努力更新中，上班族请理解。。。。。


