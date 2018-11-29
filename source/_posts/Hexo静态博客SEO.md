---
url_suffix: hexoseo
title: Hexo next主题SEO优化
date: 2018-11-21 19:10:30
tags: ['hexo','seo']
categories: 博客
keywords: next,搜索引擎优化
---
### 前言
我在大学期间使用过ZBlog、WordPress等博客系统。而Hexo与这两者的区别在于，Hexo是静态博客系统。Hexo无需数据库作为存储，也无需服务器端渲染，直接将页面以Html的格式输出给用户。全球最大男性交友社区github提供了GitPage解决方案，使得开发者可以直接将静态Html页面发布到web上。这样也避免了购买web云服务器，却只用来处理静态页面的不划算，免费的总是最好的。
### GitPage
我们可以将Hexo生成的页面push到GitPage中来发布，GitPage是Github提供的个人页面的个性化展示空间。可以通过创建一个Repository，进入设置页面激活GitPage，而后可以通过 (*用户名.github.io*) 的方式访问。[wpstan.github.io](https://www.tanrd.com)。GitPage非常友好的提供了域名映射，我们可以使用自己简短的域名来访问博客，例如本站的域名[tanRD.com](https://www.tanrd.com)。
### SEO 之 keywords
搜索引擎会在页面的<head\>标签中查询<keywords\>的mete定义，收录时会根据keywords进行索引，因此这个很有必要进行优化。打开_config.yml文件，找到keywords所在的行，添加自己希望通过搜索引擎索引的关键字。关键字个数五六个应该就差不多了，关键字个数对搜索引擎的具体影响，我也不懂。

上述改动是针对首页的关键字优化，我们还必须优化每一个文章页面的keywords,打开/themes/next/layout/_partials/head.swig文件，找到以下代码：{% asset_img 2.jpeg keywords设置代码 %}<!-- more -->
通过上述代码可以知道，next首先去页面上找keywords，没有就使用tags标签来设置keywords值，再没有就使用主题设置的keywords。我这里将keywords在页面脚手架中定义，使得在new 文章的时候，默认带上这个参数，这样就可以自定义添加keywords了。同时修改上述代码，将页面中自定义的keywords以及页面标签作为最后的keywords。{% asset_img 4.jpeg 修改keywords生成规则 %}下面是本文的顶部标题，

    title: Hexo next主题SEO优化
    date: 2018-11-21 19:10:30
    url_suffix: hexoseo
    tags: ['hexo','seo']
    categories: 博客
    keywords: ['next','搜索引擎优化']
最终在本页面中得到的keywords如下图所示：{% asset_img 3.jpeg 页面上展示结果 %}
可以看到有两个keywords属性，上方的是刚才代码拼接的结果，下方的是Hexo自带的，可以删除这个自带的。打开文件路径：/Hexo/node_modules/hexo/lib/plugins/helper/open_graph.js，将下方代码注释掉即可。
      
      if (keywords) {
        if (typeof keywords === 'string') {
          result += meta('keywords', keywords);
        } else if (keywords.length) {
          result += meta('keywords', keywords.map(tag => {
            return tag.name ? tag.name : tag;
          }).filter(keyword => !!keyword).join());
        }
      }
### SEO 之 静态URL
静态URL是指当前页面的地址不会变动，永久存在的网页地址。搜索引擎喜欢收录静态URL，以静态URL作为当前收录页面的索引。如果说当前文章页面经常改动，可想而知，搜索引擎难以收录。Hexo默认是按照日期和文章名字生成当前文章的URL地址，可以查看_config.yml文件，找到permalink这个配置。

    # URL
    ## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
    url: https://www.tanrd.com
    root: /
    permalink: :year/:month/:day/:url_suffix/
    permalink_defaults:
我这里增加了url_suffix作为URL后缀，默认是以文章的文件名拼接URL。而我文件名可能需要设置成中文，方便我查找，但是中文文字在地址栏会被HTML Encode转成html编码，将导致链接很长。因此这里我设置了一个url_suffix，主动设置文章后缀名。这个url_suffix地址在文章中不应该经常修改，特别是搜索引擎收录之后，修改地址将会导致搜索以前之前收录的链接变成死链，无法访问，非常不利于SEO。因此在创建文章的时候，手动设置了这个url_suffix后，就不要再修改了。在SEO之keywords这个章节中，我已经展示了我这个文章的定义头部分，可以看到url_suffix设置的是hexoseo。在地址栏中显示的地址是<a href="https://www.tanrd.com/2018/11/21/hexoseo/" target="_blank">https://www.tanrd.com/2018/11/21/hexoseo/</a>
### SEO 之 百度搜索引擎
由于之前百度爬虫将github搞挂了，github屏蔽了百度爬虫的抓取。但是没有关系，我们可以采用github的pages和coding的pages来发布自己的博客。coding这个公司已经被腾讯收购，是国内的一家类似github的网站，国内比较出名的git平台还有码云，我们公司最近也引入码云。

我这个域名www.tanrd.com 是通过阿里云购买的，在域名解析的时候，只需将百度爬虫解析到国内的coding page即可。{% asset_img 5.jpeg 针对百度爬虫的特殊解析 %}在解析的时候选择CNAME将百度指向coding的pages地址，这个意思是将百度爬虫的DNS解析指向国内的coding pages，我这里是wpstan.coding.me。阿里云非常方便的给我们提供了解析线路选择，有多种选择，根据自己需求来决定。除了百度指向coding的pages，其他的线路都默认指向github的pages，毕竟github的CDN很多，网速也还能接受。{% asset_img 7.jpeg 全部域名解析情况 %}
### SEO 之 SiteMap
Sitemap 可方便网站管理员通知搜索引擎他们网站上有哪些可供抓取的网页。最简单的 Sitemap 形式，就是XML 文件，在其中列出网站中的网址以及关于每个网址的其他元数据（上次更新的时间、更改的频率以及相对于网站上其他网址的重要程度为何等），以便搜索引擎可以更加智能地抓取网站。我这里搞了两sitemap，一个是sitemap.xml，可以打开<a href="https://www.tanrd.com/sitemap.xml" target="_blank">https://www.tanrd.com/sitemap.xml</a>访问；一个专门针对百度的baidusitemap.xml，地址是<a href="https://www.tanrd.com/baidusitemap.xml" target="_blank">https://www.tanrd.com/baidusitemap.xml</a>。
在Hexo中安装很容易，只需要执行如下命令即可：
``
npm install hexo-generator-sitemap --save
``
``
npm install hexo-generator-baidu-sitemap --save
``
### SEO 之 robots.txt
这个文件的作用是制定搜索引擎的爬取规则，所有正规搜索引擎都遵守该标准。新建robots.txt放在source的根目录下，修改其中内容为：
    
    User-agent: *
    Allow: /
    Allow: /archives/
    Allow: /categories/
    Allow: /tags/
    Allow: /about/
    Disallow: /js/
    Disallow: /images/
    Disallow: /css/
    Disallow: /fonts/
    
    Sitemap: https://www.tanrd.com/sitemap.xml
    Sitemap: https://www.tanrd.com/baidusitemap.xml
本站的robots.txt地址为<a href="https://www.tanrd.com/robots.txt" target="_blank">https://www.tanrd.com/robots.txt</a>
### SEO 之 nofollow
当搜索引擎在爬取文章页面的时候，如果文章中有外链，可能会将爬虫引出我们的网站，因此有必要屏蔽这种情况。首先安装hexo-autonofollowp
```npm install hexo-autonofollowp --save```
再在外层_config.yml中添加配置，将nofollow设置为true：
    
    # 外部链接优化
    nofollow:
        enable: true
        exclude:     # 例外的链接，可将友情链接放置此处
        - 'yousite'
再重新生成的时候，可以看到出站链接自动加上了一系列参数：
    
    <a href="https://github.com/XX-net/XX-Net" rel="external nofollow noopener noreferrer" target="_blank">XX-Net</a>