---
layout: post
title: '「从零开始做购物网站」第12章 暂停一下'
date: 2017-06-14 23:33
comments: true
categories: 
---

> 大家好，本系列文章主要是对我参加全栈营JDstore魔改大赛作品[「季风 & MONSOON」](http://kerzzi.logdown.com/posts/1903205-magic-change-contest-entries-monsoon)的复盘。由于本人也是新手，文章中一定有很多幼稚的错误或代码，希望大家帮忙指出，可反馈至我的邮箱kerzzi@outlook.com，我会持续完善本系列。非常感谢。
> 本系列是在全栈营JDstore教程基础之上进行魔改，本系列第1-11章节为全栈营JDstore教程，请在完成全栈营JDstore教程的基础上进行测试。

## 目标
* 为了方便debug以及方便查询各个model之间的关系，安装2个debug用gem套件（'pry'和'awesome_rails_console'）以及展示model关系的gem套件（'rails-erd'）。

## 步骤
### Step 0:建立新分支

新建分支```git checkout -b story06```

### Step 1:安装2个debug的gem套件'pry'和'awesome_rails_console'
打开Gemfile
```ruby Gemfile
group :development, :test do
  # Call 'byebug' anywhere in the code to stop execution and get a debugger console
  gem 'byebug', platform: :mri
  gem 'sqlite3'
+ gem 'pry'
+ gem 'awesome_rails_console'
end
```

执行
```
bundle install
rails g awesome_rails_console:install
```

重开```rails s```。


### Step 2: 安装展示model关系的gem套件
**安装 Graphviz 2.22+: **
* 终端机中执行 ```brew install graphviz```
* 打开```Gemfile，Gemfile```中添加 ```gem 'rails-erd'```

```ruby Gemfile
gem 'qiniu-rs'
gem 'figaro'
+ gem 'rails-erd'

group :development, :test do
```

终端机中执行 ```bundle install```

终端机中执行 ```rake erd``` 生成erd文件

**使用说明：**

* GEM自动生成的PDF格式文件，在ATOM中无法正常打开，显示一堆乱码。

* 在ATOM中找到新建的 **erd.pdf** ，右击找到 **Show in Finder**点击进入文件夹找到该文件，打开即可看到目前的model之间的关系。


### Step 3:将变更 commit 进去 git 里面。

现在可以把改动合并到 master 分支了：
```ruby zsh
git add .
git commit -m "install some awesome gem"
git push origin story06
git checkout master
git merge story06
git push origin master
```

### 效果图
![](https://ww2.sinaimg.cn/large/006tKfTcgy1fgf2w05913j31kw18g78v.jpg)


## 本章详解
>  努力更新中，上班族请理解。。。。。




