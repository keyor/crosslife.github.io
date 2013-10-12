---
layout: post
title : Chrome扩展：Better Extension Manager
tags  : [Chrome-Extension, JavaScript]
---

Chrome自带的扩展管理器不支持搜索，有时候要找一个扩展要来回找好几次，而且没有批量操作，有时候出了问题想禁用全部扩展只能一个一个的禁用掉。。于是自己写了一个扩展管理器：
![](/image/better_extension_manager.png "")  

支持搜索和批量操作。   

用到了[management api](http://code.google.com/chrome/extensions/management.html)
  
界面模仿的 [REST Console](https://chrome.google.com/webstore/detail/faceofpmfclkengnkgkgjkcibdbhemoc)，它应该是模仿的Chrome的设置页面。。
  
下载地址：<https://chrome.google.com/webstore/detail/gnnggbedbbegbdnmimdhkhkfdcikfnjl>
  
代码传到了Google Code上：<http://code.google.com/p/better-extension-manager>