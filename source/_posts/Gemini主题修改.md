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
### 静态资源压缩
可以通过安装hexo-all-minifier来进行静态资源压缩，减少网络请求时候的数据包，加快网站响应速度。可以对html、css、js、images进行压缩，即把重复的代码合并，把多余的空格去掉，用算法对images进行压缩。
``` npm install hexo-all-minifier --save ```
再在外层_config.yml中添加配置，可以分别对html、js、css、image进行处理：
    
     html_minifier:
      enable: true
      ignore_error: false
      exclude:
    
    css_minifier:
      enable: true
      exclude:
        - '*.min.css'
    
    js_minifier:
      enable: true
      mangle: true
      output:
      compress:
      exclude:
        - '*.min.js'
    
    image_minifier:
      enable: true
      interlaced: false
      multipass: false
      optimizationLevel: 2
      pngquant: false
      progressive: false

再重新生成的时候，可以看到页面上的静态资源已经被压缩，Size这一栏数据量有所减少。下面是压缩前后的传输数据量对比：{% asset_img 1.jpeg 压缩前 %} {% asset_img 2.jpeg 压缩后 %}
### 代码高亮
第一种方式：
首先安装 hexo-prism-plugin插件，执行如下命令：

    npm install hexo-prism-plugin --save
然后修改_config.yml，添加如下配置：
    
    prism_plugin:
      mode: 'preprocess'    # realtime/preprocess
      theme: 'default'
      line_number: false    # default false
      custom_css: 'path/to/your/custom.css'     # optional
相关配置字段说明如下：
* mode:
  * realtime (在浏览器实时解析代码)
  * preprocess (在node环境中先解析代码)
* theme:
  * default
  * coy
  * dark
  * funky
  * okaidia
  * solarizedlight
  * tomorrow
  * twilight
  * atom-dark
  * base16-ateliersulphurpool.light
  * cb
  * duotone-dark
  * duotone-earth
  * duotone-forest
  * duotone-light
  * duotone-sea
  * duotone-space
  * ghcolors
  * hopscotch
  * pojoaque
  * vs
  * xonokai
* line_number:
  * true (显示行号)
  * false (默认, 隐藏行号)
* no_assets
  * true (停止加载资源文件)
  * false (默认, 加载js和css文件)
 
第二种方式：直接用自带的next主题，可以在配置文件中修改自己喜欢的样式

     # Code Highlight theme
     # Available value:
     #    normal | night | night eighties | night blue | night bright
     # https://github.com/chriskempson/tomorrow-theme
     highlight_theme: night
在写博客插入代码的时候，需要指定开发语言，才会显示不同的颜色样式。

    