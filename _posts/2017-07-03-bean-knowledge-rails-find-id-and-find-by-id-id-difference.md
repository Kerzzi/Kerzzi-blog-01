---
layout: post
title: '【豆知识】Rails: find(id) 与 find_by(id: id)的区别'
date: 2017-07-03 22:01
comments: true
categories: 
---
## 目标:
> ActiveRecord的 find(id) 与 find_by(id: id) 的区别，以及各自的使用场景

## 步骤:

### Step 01:

```find(id)``` 如果没有找到记录会抛出异常，而 ```find_by(id: id)``` 不会。

一般从逻辑上来说数据肯定存在的场景使用```find(id)```，这样也能起到数据检查的功能，如果不存在自动抛出异常，说明系统有非法数据或者非法请求； 而如果数据不存在属于正常范围则可以使用```find_by(id: id)```。

## 更多ROR【豆知识】:
> 更多ROR【豆知识】请前往：https://github.com/Kerzzi/ruby_notes/tree/master/04_ROR_beans