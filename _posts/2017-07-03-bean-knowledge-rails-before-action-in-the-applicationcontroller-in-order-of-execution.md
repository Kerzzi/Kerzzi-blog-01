---
layout: post
title: '【豆知识】Rails: before_action在ApplicationController中的执行顺序'
date: 2017-07-03 22:05
comments: true
categories: 
---
## 目标:
> 如果HomeController是我们网站的首页，在里面我们定义了一个before_action，同时在ApplicationController中也定义了一个before_action，那么在请求网站首页时哪个before_action先执行？

## 步骤:

### Step 01:
ApplicationController中的先执行，因为HomeController是继承于ApplicationController的，按照代码的加载和执行顺序，父类的代码先执行。

## 更多ROR【豆知识】:
> 更多ROR【豆知识】请前往：https://github.com/Kerzzi/ruby_notes/tree/master/04_ROR_beans