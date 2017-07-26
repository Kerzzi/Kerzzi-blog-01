---
layout: post
title: '【错误记录】执行rails s出现： A server is already running 提示'
date: 2017-04-10 21:15
comments: true
categories:
---
## 错误详情
实做全栈营 招聘网站 第二周 内容的时候。打开终端输入```rails s```出现如下错误。
错误提示：```A server is already running```
但是并没有同时运行 rails server 。
![](https://ww1.sinaimg.cn/large/006tKfTcgy1fehw92f39pj31kw0mpwi4.jpg)

## 纠正措施
非常巧的是，我在开始实操之前扫了一眼今天论坛推荐的文章。
其中有篇[Anndo](http://anndo-blog.logdown.com/)同学写的错误记录，名字正是 [報錯紀錄： A server is already running](http://anndo-blog.logdown.com/posts/1683245)。

参考他的文章，1分钟解决了BUG，好痛快！

下面内容来自[報錯紀錄： A server is already running](http://anndo-blog.logdown.com/posts/1683245)。

> **解决措施：**
> **自己方式：**用終端機所給的 .pid　檔案路徑，利用指令： rm -rf <檔案路徑> 刪除該 Server.pid 檔，再重新執行 rails server。
> **助教解答：**在終端機運行指令 ```kill -9 $(lsof -wnitcp:3000 -t)```。

> 其他觀察：
> 刪除該 Server.pid 檔後，開啟 Finder 到 .pid 所在資料夾，試試看：執行 rails s 開啟 server、執行 ctrl + c 關閉。會觀察到：
> **當我們開啟 server 時，資料夾會多一個 Server.pid，利用 ctrl + c 結束 rails server 時，該檔案則會被刪除。然而，如果是在 server 運行的狀況下，直接關閉終端機，這個檔案仍然會存在。**
> 上一次關閉終端機時沒有用 ctrl + c先結束 rails server，因此少了把 .pid 檔移除掉的步驟。在下一次再開終端機要運行時，就會發生 "A server is already running" 的問題。所以務必要養成用 ctrl + c 關閉 rails server 的習慣。
