---
layout: post
title: '【提取练习】Nic老师周四答疑：基础知识补充与开发技巧 |20170413'
date: 2017-04-13 21:41
comments: true
categories:
---
这是Nic老师周四答疑的提取练习，加上我自己收集的知识。
## 放下你的纠结
非常建议你不要在小白时期这样做
1. 尝试搞懂“过多”的为什么，可以记下来，在周四提问助教。
2. 了解一个需要花上超过一天的问题。

## 小技巧让你的开发更顺手
### 1、指令下错时的自救法

> 在 Rails 中，可以使用 rails destroy 命令完成撤销操作。一般来说，下面这两个命令是相互抵消的：

> ```rails generate controller StaticPages ```
> ```rails destroy  controller StaticPages ```

> 使用命令生成模型：

> ```rails generate model User name:string email:string```
> 这个操作可以使用下面的命令撤销：

> ```rails destroy model User```
> （这里，我们可以省略命令行中其余的参数）

> 对模型来说，还涉及到撤销迁移。迁移通过下面的命令改变数据库的状态：

> ```rails db:migrate```
> 我们可以使用下面的命令撤销前一个迁移操作：

>```rails db:rollback```
> 如果要回到最开始的状态，可以使用：

> ```rails db:migrate VERSION=0```
> 你可能猜到了，把数字 0 换成其他数字就会回到相应的版本，这些版本数字是按照迁移执行的顺序排列的。

> 知道这些技术，我们就能得心应对开发过程中遇到的各种问题了。


### 2、Atom快捷篇
1. ```cmd+p``` 快速开启档案
2. 整个目录的文档中搜索单词，可以用```command + shift + F```
3. 单个文档中搜索```command + F```
4. mkdir是创建一个新的文件夹，touch是在某个文件夹里创建一个新的文件！mkdir是创建文件夹linux的指令 ，rails g 是ruby的语言。

### 3、开发必须有始有终
先打完开头和结尾。
```def end```
```<%=  %>```
```<div> </div>```
```do ... end``` 等


### 4、强化终端，除了美观更实用
1. 在终端使用```echo $0 ```命令确认你使用的是```bash``` 还是 ```zsh```
2. [安装iterm2及美化操作](http://docs.qzy.camp/docs/iterm2)
3. 在终端```cd + Tab```试试
4. [zsh使用技巧](http://kerzzi.logdown.com/posts/1703238--wish-i-had-known-of-zsh-tips)。
5. **按tab键可以在终端自动补全**
6. 在terminal中按```contl+A```可以将光标指向最前面，那修改完了，要回到最最后面的快捷键是```contl+E```。
7. **按上键是重复上一个命令**。


### 5、Git基础知识
1. ```git diff```用来对比commit之后的不同之处。
2. 按照rails101做joblisting，把job输成了group，结果产生的文件都是group名称。我删除这个分支，可是那些group的文件夹仍然存在。怎么删掉？答：一个小技巧（**删除上一次commit到目前为止的所有变动**），先```git status```,看看哪些文件被改动了，然后```git add .```将改动内容放入暂存区，接着```git stash```让进入暂存区的内容进入缓存状态, 然后```git stash clear```,这样就清理了stash状态的改动内容，然后```git status```就可以看到已经没有改动的文件了。
3. 老师在view里写路径的时候，比方说，```new_admin_job_path(@job)```有的时候需要后面挂这个@job,有的时候不需要，每次我都要靠着红字报错才能修改对。该怎么理解这个呢?答：**不使用job时，说明是路径是一个jobs集合页面。指定的话就是一个具体的job页面。**
4. 如何回到上一个git commit时的文件存档？答：```git checkout . ```**放弃所有文件的修改（只对已经被 git 追踪过的文件有效），当你的文件被改烂了而又不想保存变更的时候，这个命令可以让项目的状态回到上一次 commit 的状态。
5. 不小心在系统根目录使用了 ```git init```，用什么命令可以撤销？答：你可以试试 ```rm -rf .git```，其实执行```git init``` 就是在系统层面上建立了一个隐藏的文件夹，用来追踪你的改动，所以一般不建议在根目录使用这个命令。这个命令是删除命令，可以删除文件夹。
6. 回到上一个git 存档时的文件状态。使用命令```git reset --hard HEAD~ ```这个会删除本地的更改,或者先```git log```查看有几个git, 然后使用```git reset --hard HEAD~1```回到排在第一个位置的状态。后面的数字根据需要改变。
![](https://ww2.sinaimg.cn/large/006tKfTcgy1feldwp0w1kj31kw0zhgre.jpg)

下文来自[nfreeness师兄](http://nfreeness.logdown.com/posts/2016/10/01/938720)：
> **What is git?**
> git 是 Linux 之父 Linus Torvalds 开发出来的一套代码版本控制系统。你可以把 git 想像成游戏时存档的工具，每当程式开发到一定的段落，就使用 git 存档一下。

> **git config**
> 就像是首次玩游戏一样，你必须设定游戏玩家角色叫什么。在安装完 git 之后我们必须设定两个参数：
> ```git config --global user.name "Jone Doe"```
> ```git config --global user.email johndoe@example.com```
> 这样以后进行存档时，就会纪录你是这次进度的存档者。

> git init
> 默认情况下 git 不会对我们的项目进行版本控制，所以每次新建一个项目之后，需要使用 git init 这个命令，来告诉 git 对我们的项目进行追踪，但是这个命令只要使用一次就可以了，之后在这个项目里，你只要 ```git add . ``` 添加变动 和``` git commit``` 保存变动即可。
> 注意：千万不要在系统根目录下使用 git init，否则你整个电脑的所有文件都会被记录！

> git status
> ```git status``` 会告诉你 git 追踪到的所有新增、修改、删除的文件。
> 比如我们修改了 ```gemfile``` 和 ```README.md``` 两个文件，
> 然后我们输入 ```git add README.md```，再输入 ```git status```
> 你会发现 ```README.md``` 变成绿色，```gemfile``` 档案还是红色。
> 这表示我们已经将 ```README.md```设定为预备储存进度的文件”。

> git add
> ```git add foo.txt``` 把名为 foo.txt 的文件加进追踪修订。
> ```git add .``` ("git add dot") 把当前项目文件夹里（即 .）所有新文件和改过的文件加进追踪修订，但“保留”你删掉的文件。
> ```git add -A``` 全部加进追踪修订，包括删掉的文件。“把删掉的文件加进来”听起来很奇怪，但如果你把版本控制系统想像成是追踪“修改”的工具，或许就会懂了。多数人用 git add . ，但 git add -A 会比较安全。

> git commit
> ```git commit``` 告诉 git 真的要执行你叫它做的事。
> 分成 add 和 commit 两个步骤的好处是，如此你就可以把多个修改合并在一起。
> ```git commit``` 的用法是 ```git commit -m “描述”``` ，记得要加 -m 这个参数，后面的 ”描述” 里，是指你这一次commit 到底做了哪些修改，至少你不能每次都用 Added all the things ，不然以后你查看记录的时候每一次都是 Added all the things，就会不知道自己做了什么。

> 前方乱入：```git reset --hard HEAD~``` **快速回到上一次 commit 状态。**

> git push
> 把你本地有经过 git commit 过的代码推送到远端 github 或是 heroku之类的储存库上面去。
> ```git push --all origin``` 接将所有分支全 push 到 github 上。
> ```git push origin master``` 将本地的 master 分支 push 到 github 上。

> git pull
> 在多人合作开发的项目中需要经常将其他人 push 到 github 上面的代码 “拉” 到本地，这样才能同步变动。
> ```git pull origin develop``` 将 github 的 develop 分支 pull 到本地的当前分支。

> git clone
> ```git clone``` 的作用是将 github 上面的项目（别人的或自己的）克隆到本地。
> **注意，不要在一个已经 git init 过的目录下进行 git clone。**

> ```git clone https://github.com/twbs/bootstrap.git``` 将 github 上 twbs 这个用户的 bootstrap 项目代码克隆到你的电脑上（这会建立一个名为 bootstrap 的文件夹）。
> ```git clone https://github.com/twbs/bootstrap.git my_bootstrap``` 和上个命令一样，但是这里会创建一个名为 my_bootstrap 的文件夹用于存放代码。
> ```git clone https://github.com/twbs/bootstrap.git -b v3-dev``` 这是只将 v3-dev 这个分支 clone 下来。

> git branch
> git 除了可以保存项目进度，还可以通过分支（branch）来管理不同的开发状态，比如 master 分支是主分支，用于发布正式版本，一般不直接在这上面进行修改; develop 分支则是开发用的；还可以在 develop 的基础上延伸每一功能模块分支，当修改好了之后再 merge 到 develop 分支。

> git branch 查看项目有多少个分支，且当前处于哪个分支。
> ```git branch fixbug``` 以当前分支为副本，建立一个名为 fixbug 的分支。
> ```git branch -D fixbug``` 删除名为 fixbug 的分支（当前分支必须是 fixbug 之外的其他分支才能删除）。

> git checkout
> ```git checkout . ```**放弃所有文件的修改（只对已经被 git 追踪过的文件有效），当你的文件被改烂了而又不想保存变更的时候，这个命令可以让项目的状态回到上一次 commit 的状态**，当然也可以用 ```git checkout 文件名``` 只放弃某个被改烂的文件。
> ```git checkout fixbug``` 从当前分支切换到 fixbug 分支。
> ```git checkout -b fixbug02``` 以当前分支为副本建立一个 fixbug02 分支，并直接切换到新建的分支( 用 git branch 新建分支不会切换到新分支 )。

> git stash
> ```git stash``` 将所有修改缓存起来（而不是保存），这样你就可以 checkout 到其他分支而不需 commit 。
> ```git stash apply``` 将 git stash 缓存的文件重新调出来。

> git merge
> ```git merge develop``` 假设当前在 master 分支下，这个命令的意思是将 develop 分支里的内容合并到 master 分支。

> git log
> ```git log``` 查看过去的 commit 记录，包含了 commit 的 SHA1值（ commit id ）、修改人信息、修改时间、备注信息等。**在显示 log 的状态下，按 Q 键退出查看**。

> git diff
> ```git diff``` 用于查看“代码”的增删改，它与 ```git status``` 不同（查看“文件”的增删改），**在 commit 之前，通过 git diff 查看代码的具体变动，比如减少了某一行代码或者新增了某一个行代码**。**同样是按 Q 键退出查看**。

[Git入门常用命令](http://zencode.cn/2012/12/gitbasis/)
[Git的各种Undo技巧](https://tonydeng.github.io/2015/07/08/how-to-undo-almost-anything-with-git/)

### 6、Heroku是什么
1. 如何删除Heroku上的应用。
