---
layout: post
title: '【豆知识】git小技巧：暂存自己正在开发的代码'
date: 2017-07-03 21:57
comments: true
categories: 
---
## 目标:
> 正在一个分支上开发代码，但是有人（还会有谁）让你改个其他分支上的bug，但是你现在的修改还不能commit，怎么办?

## 步骤:

### Step 01:

这时就可以使用stash：```$ git stash```。
stash将会暂存你当前(working tree)的修改，然后你就可以切换到其他分支工作，最后当你回到自己的分支时执行:```$ git stash pop```。这样就把你之前暂存的修改重新取出来了。

## 更多ROR【豆知识】:
> 更多ROR【豆知识】请前往：https://github.com/Kerzzi/ruby_notes/tree/master/04_ROR_beans

