---
layout: post
title: '【豆知识】git小技巧：如何恢复已经删除的文件，包括本地和远程的'
date: 2017-07-03 21:55
comments: true
categories: 
---
## 目标:
> 不小心或者故意删除了文件，想要恢复怎么办。

## 步骤:
### Step 01:

这要区分两种情况，一种是刚刚删除还没有提交(commit)的情况，还是已经commit了:

如果还没有commit：```$ git checkout path/to/file```

如果已经commit了，这时就稍微麻烦一点，需要先找到删除该文件的commit hash：
```
$ git rev-list -n 1 HEAD -- path/to/file
6493e8db891dd643540e22b54d44c45f286f5cff
# 上面会打印出影响该文件的hash，然后再恢复该文件：
$ git checkout 6493e^ path/to/file
```

这样就可以了，不要忘了上面的```hash```后面有个```^```，这是查找上一次的```commit hash```，因为上面输出的hash是你删除该文件的hash， 要恢复该文件的话就要找到删除之前的状态，因此是上一次的hash。

## 更多ROR【豆知识】:
> 更多ROR【豆知识】请前往：https://github.com/Kerzzi/ruby_notes/tree/master/04_ROR_beans