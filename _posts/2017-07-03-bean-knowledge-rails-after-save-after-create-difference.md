---
layout: post
title: '【豆知识】Rails: after_save与after_create的区别'
date: 2017-07-03 22:04
comments: true
categories: 
---
## 目标:
> after_save和after_create是模型中常用的回掉方法，它们的区别是什么。

## 步骤:
### Step 01:
after_save是在数据insert和update时都会触发，而after_create只有在insert时触发。

## 更多ROR【豆知识】:
> 更多ROR【豆知识】请前往：https://github.com/Kerzzi/ruby_notes/tree/master/04_ROR_beans