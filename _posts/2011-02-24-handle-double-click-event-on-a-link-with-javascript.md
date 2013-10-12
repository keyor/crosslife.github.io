---
layout: post
title : JavaScript中如何处理双击链接事件
tags  : [JavaScript]
---

JavaScript中有一个ondblclick事件，它在对象被双击的时候发生
  
但是当我们把它用在链接上时情况就有了一些变化，在我们双击链接时，我们会发现页面跳转到了链接所指的页面。为什么呢？双击相当于两次间隔很短的单击，所以双击链接时首先会触发链接的onclick事件，而链接的onclick事件默认的动作是跳转到链接指向的地址。所以当我们把双击链接的动作用慢镜头放出来时实际是这样的：第一次点击后浏览器开始跳转到链接所指的页面，这时发生了第二次点击，浏览器又重新开始跳转到链接所指的页面（准确的说这是页面加载较慢而且双击间隔较短时的情况）。总之，双击链接的最终结果是页面跳转到了链接所指的地址。
  
  
如何解决这个问题呢？显然我们不能再使用ondblclick事件，我的解决办法是：当第一次点击发生时开始计时，然后，如果在一个较短的时间内(比如几百毫秒)又发生了一次点击，我们认为发生了双击，然后采取一定的动作，并把计时器清零；如果在这个间隔内没有发生另一次点击，我们认为这是一次单击，于是跳转到链接所指的地址。代码如下：

{% highlight javascript %}
  document.getElementById("google").onclick = function(){
      var link = this.href;
      if(this.clickTimeout){
          // 双击
          clearTimeout(this.clickTimeout);
          this.clickTimeout = null;
          alert(link);
      }
      else{
          // 单击
          var elem = this;
          this.clickTimeout = setTimeout(function(){
              // 跳转到相应网址
              elem.clickTimeout = null;
              window.location.href = link;
          }, 250);
      }
      //阻止链接onclick时的默认行为
      return false;
  };
{% endhighlight %}
  
可以到这里查看运行后的效果： <http://jsfiddle.net/wong2/Rfedt/> (在此推荐一下[jsfiddle](http://jsfiddle.net "")这个网站，可以在线编写、运行js程序，可以使用JQuery等JS库，而且可以和别人共享)
  
最后说一下程序里判断是双击或单击的那个间隔怎么选择，我写了一个页面来测试自己双击鼠标的时间间隔，结果是我大概是150-200毫秒之间，所以保险起见，选择了250毫秒。
大家也可以到[這裡測下](http://wong2js.googlecode.com/svn/trunk/get_dblclick_interval.html "")
  
对了，还没在IE里测试。。


