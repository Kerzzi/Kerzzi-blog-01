---
layout: post
title: '【错误记录】如何修改Model的栏目名称？'
date: 2017-04-14 21:38
comments: true
categories: 
---
微信群里讨论的一个问题，比较实用，参考[陈俊鸿同学的博客](http://raimonfuns-blog.logdown.com/)。

> 动作
> 创建表的时候，手残把栏目名敲错了```rails g model post titlr:string content:text```。

> 分析
> 把栏目名从```titlr```改成```title```

> 解决方法
> step1:
> 执行```rails g migration FixColumnName```创建migration

> step2:
> 修改```db/xxxxxx_fix_column_name.rb```

> ```
  class FixColumnName < ActiveRecord::Migration[5.0]
   def change
     rename_column :posts, :titlr, :title
   end
 end
 ```


> step3:
> 执行```rails db:migrate```，done！

**参考链接：**
[How can I rename a database column in a Ruby on Rails migration?](http://stackoverflow.com/questions/1992019/how-can-i-rename-a-database-column-in-a-ruby-on-rails-migration)
