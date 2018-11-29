---
title: hexo的gitment评论报错
tags:
  - hexo
  - gitment
categories: 博客
keywords: '评论,报错'
date: 2018-11-28 19:56:41
url_suffix: hexogitmenterror
---
### Gitment报错
今天发现我博客hexo采用的github issue的评论系统gitment出现了以下报错：
所有评论
Error: API rate limit exceeded for 107.178.194.84. (But here's the good news: Authenticated requests get a higher rate limit. Check out the documentation for more details.)
解决方案：无需解决
原因：这是github的自我保护机制，防止未登陆用户重复调用API。点击下方github登陆按钮登陆后，错误消失。


