---
layout: post
title: '【错误记录】少写个! 感叹号，造成逻辑判断错误'
date: 2017-04-09 17:00
comments: true
categories:
---
## 问题描述
实作[Rails实战：招聘网站时](https://fullstack.xinshengdaxue.com/posts/546)6-2时，在Step 9 出现已添加管理员权限的人员依然无人登录后台的问题，出现提示消息```You are not admin```

## Debug

仔细检查了下，原来是管理员权限判断时，if语句中少个```!```（逻辑非）
```ruby app/controllers/application_controller.rb
class ApplicationController < ActionController::Base
  protect_from_forgery with: :exception

  def require_is_admin
    if !current_user.admin? //刚开始少了个！感叹号
      flash[:alert] = 'You are not admin'
      redirect_to root_path
    end
  end
end
```
