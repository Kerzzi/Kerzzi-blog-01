---
layout: post
title: '【错误记录】拼写错误、代码补全导致错误以及重启治百病'
date: 2017-04-15 10:58
comments: true
categories:
---
对今天上午遇到的BUG及相应处理措施进行复盘。
## 拼写或少写符号错误

应为:style,少个冒号
![](https://ww4.sinaimg.cn/large/006tKfTcgy1fen6j6l1auj31kw1qugpz.jpg)
拼写错误：save写成sava
![](https://ww2.sinaimg.cn/large/006tKfTcgy1fen6jecf0yj31kw1qu44t.jpg)


## 代码补全导致错误
由于我启用了代码补全功能，偶尔发生补全内容不是我想要的情况。
![](https://ww1.sinaimg.cn/large/006tKfTcgy1fen6jp8g77j31kw1bfq8o.jpg)
![](https://ww4.sinaimg.cn/large/006tKfTcgy1fen6jwq06ej31kw1quwkd.jpg)


## 重启治百病
重启电脑、重启```rails s```真的可以解决至少100个BUG。

招聘网站 11-2 step8完成后出现如下错误，实际上文件中并没有错误，必须重启```rails s```，才能显示效果。

```cmd + /``` 是atom中html文件的注释功能快捷键。

![](https://ww1.sinaimg.cn/large/006tKfTcgy1fen6k25tw7j31kw1quaed.jpg)
