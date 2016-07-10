title: githubpages+jekyll博客
date: 2014-04-13
tags:
---

题记：周末花了很长时间，基于gitpages+jekyll搭建博客，记录下其中一些过程

**一、首先得理解：**

1.Github Pages 是什么 ?  
GitHub Pages are public webpages freely hosted and easily published through our site。可以认为是用户编写的、托管在github上的静态网页，是个免费的host服务。

2.Jeklly 是什么 ?  
>Jekyll从核心上来说是一个文本转换引擎。工作原理是：你输入一些用自己喜爱的标记语言格式书写的文本，可以是Markdown、Textile或纯粹的HTML，它将这些文本混合后放入一个或一整套页面布局当中。在整个过程中，你可以自行决定你的站点URL的模式、以及哪些数据将被显示在页面中，等等。这一切都将通过严格的文本编辑完成，而生成的Web界面则是最终的产品。  

3.github pages与jeklly关系  
Jekyll is the engine behind GitHub Pages. Every GitHub Page is run through Jekyll when you push content to a specially named branch within your repository.  
即：提交到github-pages的内容会经过jeklly处理，转换为静态的html，但结构必须符合jeklly的要求
>参见：www.soimort.org/posts/101/ <基本结构> 部分

**二、动手操作**  

1.请认真阅读，非常清楚  
http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html  
注：例子中使用project-site方式，user-site方式会更简单点，无需分支gh-pages。请看https://pages.github.com/
> [help文档](https://help.github.com/articles/using-jekyll-with-pages) 请注意2点：  
a、For User Pages, use the master branch in your username.github.io repository. For Project Pages, use the gh-pages branch in your project's repository.  
b、Because a normal HTML site is also a valid Jekyll site, you don't have to do anything special to keep your standard HTML files unchanged.

2.Jeklly是否一定要在本地安装 ?  
> We highly recommend installing Jekyll on your computer to preview your site and help diagnose troubled builds before publishing your site on GitHub Pages.  
目的是为了方便调试问题，所以不是必须的。安装比较繁琐。  

推荐：找一个满意的模板，clone下来简单修改，很少需要在本地调试。https://github.com/jekyll/jekyll/wiki/sites

**三、其它**

1.当提交后，一直不能生效，一般是因为jeklly处理错误。可以查看邮箱确认：github会发一封 'Page build failure'

2.推荐markdown编辑器：haroopad

3.hexo与jeklly比较
![](http://git-blog-img.qiniudn.com/2014-04-13-gitpages-jekyll-blog-compare.jpg)

4.在编辑器样式与最终样式不一致 ? 切记：最终展现都是转换为html效果
a、首先：检查转换的html标签是否正确，如：\> 是否转换为 blockquote 标签  
b、同时：直接打开github仓库的源码，核对样式，如：https://github.com/fatmind/fatmind.github.io/blob/master/_posts/2012-08-25-hello-world.md  
c、若a无问题，则肯定是css样式不一致  
d、若a有问题，很可能是编辑器的markdown转换与jeklly使用的不兼容，按理概率很小，markdown作为标准的定义，则说明编辑器有问题（jeklly已经有太多人在使用，bug概率太小）  
e、不支持 'fenced code blocks'  
http://stackoverflow.com/questions/10759577/underscore-issues-jekyll-redcarpet-github-flavored-markdown， 配置_config.yml
```ruby
markdown: redcarpet2
redcarpet:
  extensions: ["no_intra_emphasis", "fenced_code_blocks", "autolink", "tables", "with_toc_data"]
```
---
*备注：实在忍受不了，上传上去后才发现有问题jeklly转换失败，已经在用 hexo*
