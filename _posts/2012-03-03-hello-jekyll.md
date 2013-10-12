---
layout: post
title : Hello Jekyll
tags  : [blog]
---

用了一晚上从wordpress换到了jekyll.  

[jekyll][jekyll-home]是一个静态博客生成器,其作用是根据模板文件(可以理解为HTML模板)和用markdown等语言写的文字生成一个博客的静态HTML文件,然后把这些文件随便放到哪台服务器上就可以了.  

为什么我要从wordpress换到jekyll? 主要有以下几个原因:  

* 一直不喜欢wordpress的编辑器  
* wordpress的安全问题,之前我的博客就不知道怎么被黑了,文章里都被插了广告链接,费了很大的力气一一删除,过了几天又出来了,禁用插件升级wordpress都没用.而静态博客系统基本不会有这方面的问题.  
* 可以用[markdown][markdown-url]写博客  
* 可以用自己喜欢的编辑器写博客,比如vim  
* 可以离线写博客,然后在本地预览,合适之后再提交到服务器就可以了.比如我现在就在寝室断电断网的情况下写这篇文章...    

现在我的博客托管在GitHub上([源码][blog-src]),GitHub一个很好的功能就是托管个人页面,只要建立一个id.github.com的代码仓库,然后把jekyll目录放上去就可以了.

现在每次发表新文章的流程就成了: 在本地用vim写好文章内容->本地预览一下->git push到github->github自动生成静态HTML文件. 非常的方便,为此我都放弃了原来的在VPS上托管博客的打算...

[blog-src]:     https://github.com/wong2/wong2.github.com
[jekyll-home]:  http://jekyllrb.com/
[markdown-url]: http://daringfireball.net/projects/markdown/
