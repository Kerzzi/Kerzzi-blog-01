---
layout: post
title: '【豆知识】Rails: 怎样统一admin后台管理的管理员验证逻辑'
date: 2017-07-03 21:59
comments: true
categories: 
---
## 目标:
> 网站一般都有后台管理的功能，同时需要有验证管理员身份的逻辑，如果我们有很多不同的Controller都属于后台管理的逻辑，那么应该怎样组织代码来统一管理员验证的逻辑？

## 步骤:

### Step 01:

建立一个父Controller比如是```Admin::BaseController```：

```
class Admin::BaseController < ApplicationController

  before_action :auth_admin

  private
  def auth_admin
    # ....
  end

end

class Admin::Users < Admin::BaseController
end

class Admin::Posts < Admin::BaseController
end
```

## 更多ROR【豆知识】:
> 更多ROR【豆知识】请前往：https://github.com/Kerzzi/ruby_notes/tree/master/04_ROR_beans
