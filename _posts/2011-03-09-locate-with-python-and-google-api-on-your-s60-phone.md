---
layout: post
title : S60手机上利用Python和Google API实现基站定位
tags  : [Python, Google, S60]
---

首选说一下基站定位的原理。

当前常用的基站定位是根据手机当前所连接的基站的位置来确定手机的位置。我们要利用手机进行通信，首先要连接到附近的某个基站从而接入网络，而每个基站都有一个唯一的id(CellID)，这个CellID是可以在程序里获得的，如果我们有一个存有基站id和地理位置的对应表的数据库的话，就可以根据某个CellID获取这个基站的位置，然后用这个位置来表示手机当前的位置（可以看出，定位的精度是和基站的覆盖范围有关的，在一些基站密集的地方，定位精度就会高些。）  

显然我们一般人是搞不到的这个数据库的，好在Google提供了API可以让我们使用他们的数据，那Google是怎么得到这些数据的呢？

据猜测是这样的：当你在有GPS功能的手机上使用Google Map客户端进行定位时，Google会把根据GPS得到的位置和当前连接的基站的id记录下来，从而得到基站id和位置的对应数据。

下面是具体实现步骤：  

首先要在手机上安装[PyS60](http://sourceforge.net/projects/pys60/files/)，这个就不详细说了。  

PyS60里提供了一个location模块，这个模块的只有一个方法，就是location.gsm\_location()，它会返回以下四个值：  
`Mobile Country Code(mcc), Mobile Network Code(mnc), Location Area Code(lac), Cell ID(cid)`

再看Google提供的API：  

向 http://www.google.com/loc/json 这个地址POST类似下面的数据即可：
{% highlight javascript %}
    {
        "host": "maps.google.com",
        "home_mobile_country_code": 460,
        "home_mobile_network_code":0,
        "radio_type": "gsm",
        "cell_towers":[{
            "cell_id":5983,
            "location_area_code":28712,
            "mobile_country_code":460,
            "mobile_network_code":0,
        }]
    }
{% endhighlight %}

  
返回的数据如下：
{% highlight javascript %}
    {
        "location":{
            "latitude":30.513959,
            "longitude":114.419156,
            "accuracy":888.0
        },
        "access_token":"2:vY6FFDaZ11l_VyfQ:nT3L6kGnYakaxwYZ"
    }
{% endhighlight %}
  
API的具体描述见这里：<http://code.google.com/apis/gears/geolocation_network_protocol.html>

最后实现的代码如下：
{% highlight python %}

import urllib, json
import location

def getLocation():
    mcc, mnc, lac, cid = location.gsm_location()
    location = getLatLonFromGoogle(mcc, mnc, lac, cid)
    return location

def  getLatLonFromGoogle(mcc, mnc, lac, cid):
    data = {
        "version": "1.1.0" ,
        "host": "maps.google.com",
        "home_mobile_country_code": mcc,
        "home_mobile_network_code":mnc,
        "radio_type": "gsm",
        "cell_towers":[
            {
                "cell_id":cid,
                "location_area_code":lac,
                "mobile_country_code":mcc,
                "mobile_network_code":mnc,
            }
        ]
    }
    str_data = json.write(data)
    response = urllib.urlopen("http://www.google.com/loc/json", str_data).read()
    json_response = json.read(response)
    return json_response['location']

if __name__ == '__main__':
    print getLocation()

{% endhighlight %}
  
(PyS60里没有json模块，我从网上找到了别人写的一个PyS60上可以用的json模块：<http://www.mobilenin.com/mobilepythonbook/json.py>
把这个文件放到手机的E:/Python/lib 下即可在程序里调用了。)  

把上面的代码保存为location.py，然后放到手机里，运行如图：  
![](/image/pys60.png "") 

准确度如何呢？我去Google Earth上测了一下，如下：  
![](/image/google_earth_locate.png "") 

精度还凑合吧。。。
