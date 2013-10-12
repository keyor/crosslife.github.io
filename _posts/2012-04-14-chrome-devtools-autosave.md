---
layout: post
title: Chrome DevTools AutoSave
tags: [Chrome, JavaScript]
---

今天意外发现的一个好东西，介绍一下。  

我们知道，在Chrome Developer Tools里可以编辑元素的css属性，然后实时的看到显示效果的变化。经常性的一个流程就是:  
在开发者工具里设置属性并看效果->确定之后切换到编辑器，修改为刚刚确定的属性值，然后保存文件->再切换回Chrome，刷新页面看新效果。  
如果在Chrome开发者工具里做的修改能直接被保存到代码文件里岂不是很方便？Chrome DevTools AutoSave就是做这件事的。

###安装
这个应用包括两个部分，Chrome扩展和服务器：

#####安装Chrome扩展：  
由于这个扩展用到了实验性API，所以要先去chrome://flags/里启用 Experimental Extension APIs，然后重启Chrome
接着安装扩展：<http://userscripts.ru/js/chrome-devtools-autosave/latest.crx>

#####安装服务器：
首先安装[node.js](http://nodejs.org/)  
然后在终端执行 `sudo npm install -g autosave` 安装autosave服务器端

###使用
在终端里执行 `autosave`，会看到`running on http://127.0.0.1:9104`的提示  
然后在Chrome里以`file://`的形式打开一个本地的html页面，用开发者工具修改一些css(据说js也可以)  
OK，改动已经被保存到代码文件里了

如果想对不是以`file://`的形式打开的页面生效，则需要一些配置，下面以`http://localhost`为例:  
首先创建一个文件routes.js(放哪都可以)，然后在里面写上下面的内容(路径什么的自己改吧):  
{% highlight javascript %}
exports.routes = [
    {
        from: new RegExp('^http://localhost/'),
        to: '/home/wong2/codes/www/'
    }
];
{% endhighlight %}

然后去Chrome扩展管理页面(chrome://chrome/extensions)找到刚才装的DevTools autosave，打开它的选项页，添加一条新规则，如图：

![autosave extension](http://commondatastorage.googleapis.com/wong2blog/autosave.png)

然后在终端里执行 `autosave --config /path/to/your/routes.js`，就OK了。

原理大致是当在Chrome开发者工具里做出修改的时候会触发一个[onResourceContentCommitted][]事件,
[onResourceContentCommitted]: http://code.google.com/chrome/extensions/dev/experimental.devtools.inspectedWindow.html#event-onResourceContentCommitted
扩展监听这个事件，然后把新内容POST到服务器端监听的那个端口，由服务器端修改文件内容。

###链接
Chrome DevTools Autosave: <https://github.com/NV/chrome-devtools-autosave>  
Chrome DevTools Autosave Server: <https://github.com/NV/chrome-devtools-autosave-server>
