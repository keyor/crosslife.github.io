---
layout: post
title : 有趣的排序算法：SleepSort
tags  : [Python, Algorithm]
---

昨天在人人和白云上都看到有人发了关于这个算法的帖子，看了下，确实很有趣。   
  
原帖应该是在：<http://dis.4chan.org/read/prog/1295544154>，不过这一会我这里访问不了这篇帖子。

作者说：
> Man, am I a genius. Check out this sorting algorithm I just
> invented.
  
然后贴出了下面的shell脚本：
{% highlight bash %}

    #!/bin/bash
    
    function f() {
        sleep "$1"
        echo "$1"
    }
    
    while [ -n "$1" ]
    do
        f "$1" &
        shift
    done
    wait
    
    example usage: ./sleepsort.bash 5 3 6 3 6 3 1 4 7

{% endhighlight %}

  
基本思想就是对一组数：
a1, a2, ..., an，创建n个线程，每个线程sleep相应的时间，然后将其输出，这样最后输出来的就是有序的了。。
显然，对大数、负数是很有问题的，不过想法很有趣。

下面是我写的Python版：
{% highlight python %}
    
    from __future__ import print_function
    from  threading import Timer
    
    list_to_sort = [8, 2, 4, 6, 7, 1]
    
    for n in list_to_sort:
        Timer(n, lambda x: print(x), [n]).start()

{% endhighlight %}
  
------------------------------割一下--------------------------------------
  
看到这个算法之后，我想到了编程珠玑第一章讲到的bitmap排序方法，
其思想是对一组数： a[0], a[2], ... a[n-1]，申请max(a[i])个二进制位，每位默认设成0，为了方便，我们用BitMap表示这些位的集合，
比如BitMap[i]就是第i位，然后从a[0]-\>a[n-1]遍历一遍，把BitMap[a[i]]设为1，然后从BitMap的第一位开始往后遍历，如果这一位是1，就把当前位置输出，这样一直遍历到最后，得到的就是一组有序的输出。
  
其实这两种算法在本质上是一样的，只不过一个是切分时间(SleepSort法)，一个是切分空间(BitMap法)。
