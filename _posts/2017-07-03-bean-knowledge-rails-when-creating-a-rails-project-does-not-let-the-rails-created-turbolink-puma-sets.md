---
layout: post
title: '【豆知识】Rails: 创建Rails项目时不让Rails自动创建Turbolink，Puma等设置'
date: 2017-07-03 22:06
comments: true
categories: 
---
## 目标:
> 在创建新的Rails项目时，不想使用Turbolink，Puma等框架，怎样阻止它?

## 步骤:

### Step 01:

```$ rails new new_project --skip-test-unit --skip-bundle --database=postgresql --skip-turbolinks --skip-puma```

上面的命令会：

* 去掉Rails默认的test_unit
* 不自动运行bundle
* 指定数据库为postgresql，不使用默认的sqlite3
* 不使用turbolink
* 不使用puma


## 更多ROR【豆知识】:
> 更多ROR【豆知识】请前往：https://github.com/Kerzzi/ruby_notes/tree/master/04_ROR_beans