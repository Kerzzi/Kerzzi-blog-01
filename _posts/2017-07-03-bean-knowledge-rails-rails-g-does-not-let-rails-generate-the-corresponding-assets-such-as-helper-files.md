---
layout: post
title: '【豆知识】Rails: 使用rails g方法时不让rails生成对应的assets, helper等文件'
date: 2017-07-03 22:06
comments: true
categories:
---
## 目标:
> 很多时候我们想自己来手工创建对应的asset，helper和test文件，但是默认情况下Rails会自动帮我们创建，怎样阻止它？

## 步骤:

### Step 01:

修改```config/application.rb```文件，添加下面的内容：

```
config.generators do |generator|
    generator.helper false
    generator.assets false
    generator.view_specs false
    generator.test_framework false
    generator.skip_routes true # 同时跳过路由的自动设置
  end
end
```

## 更多ROR【豆知识】:
> 更多ROR【豆知识】请前往：https://github.com/Kerzzi/ruby_notes/tree/master/04_ROR_beans
