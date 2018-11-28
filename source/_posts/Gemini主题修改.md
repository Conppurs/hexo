---
url_suffix: geminimodify
title: Hexo Next - Gemini主题修改
date: 2018-10-19 17:38:26
tags: ['hexo','gemini']
categories: 博客
keywords: 修改,个性化
---
### 前言
在Hexo的模板中选择了Next主题，在Next主题中看中了Gemini风格的页面。但是有一些小点不符合本人审美，特意修改了一下，以满足我自己的视觉体验。
### 页面宽度修改
chrome浏览器F12查看到页面的内容宽度占比为75%，找到themes中的next文件夹，进入/source/css/_variables/目录，打开Gemini.styl文件。可以看到以下内容：
{% asset_img gemini_styl.jpg 将75%修改成90%即可增大可视区域。%}<!-- more -->也可以设置$body-bg-color，改变页面背景色。
### 页面底部主题信息删除
打开themes中的next文件夹，找到_config.yml文件，修改footer下面的配置为false即可。
                                             
      copyright:
      # -------------------------------------------------------------
      # Hexo link (Powered by Hexo).
      powered: false
    
      theme:
        # Theme & scheme info link (Theme - NexT.scheme).
        enable: false
        # Version info of NexT after scheme info (vX.X.X).
        version: false
### 页面顶部黑色线条删除
打开themes中的next文件夹，找到/source/css/_common/components/header/headerband.styl文件，删除background样式即可。

      .headband {
        height: $headband-height;
        background: $headband-bg;
      }