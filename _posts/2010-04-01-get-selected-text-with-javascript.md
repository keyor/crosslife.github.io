---
layout: post
title : JS获取页面中被选中的文字
tags  : [JavaScript]
---

这是gogo给我的一个任务，以前也见过这样的应用，比如金山词霸的网站有个即划即译的功能，就是鼠标划词然后自动加载翻译，如图：
![](/image/select_word.jpg "")

这个主要要考虑浏览器的兼容性。  

在IE中，要使用
{% highlight javascript %}
document.selection.createRange().text  
{% endhighlight %}
在 Firefox、Chrome、Opera中则使用 
{% highlight javascript %}
window.getSelection()  
{% endhighlight %}

所以为了兼容性，要判断一下是哪种浏览器然后调用对应的方法。为了使代码简练点，我用了一个三元运算符
{% highlight javascript %}
var sText = window.getSelection ? window.getSelection() : document.selection.createRange().text;
{% endhighlight %}
