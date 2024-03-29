---
layout: post
title: '【豆知识】git小技巧：怎样把一个commit提交的内容应用到另外一个分支上'
date: 2017-07-03 21:49
comments: true
categories: 
---
## 目标:
> 你正在一个分支上写几个新需求，这时有人（还能有谁）过来跟你说某个已经写好的功能能不能先上线；
但是你还有一些功能只做一半，这个分支还不能提交上线，怎么办？

## 步骤:

### Step 01:

你心里默默问候了一下上面那个谁的亲人，然后飞速的敲下了以下命令：
``` $ git log --grep="feature name" ```。

首先通过上面的命令过滤出你哪些commit是关于这个功能的，你需要把上面grep后面的参数换成你提交时的commit信息，当然你也可以直接通过```git log```不加任何参数找到你的这些commit

### Step 02:

然后切换到你需要上线的分支比如master，当然切换之前可能你需要先通过```git stash```或者```commit```你当前的修改，然后：

```
$ git cherry-pick 373996b
$ git cherry-pick 1ff2abx
...
```
上面依次使用```cherry-pick```命令把对应的```commit hash```所对应的修改应用到当前分支，```commit hash```为你上面通过```git log```查找出来的```commit hash```。

> 这里需要注意的是你执行```cherry-pick```命令的顺序为```git log```打印出来的反向顺序，因为你要从最老的提交应用到最新的，一定要注意。

## 更多ROR【豆知识】:
> 更多ROR【豆知识】请前往：https://github.com/Kerzzi/ruby_notes/tree/master/04_ROR_beans

