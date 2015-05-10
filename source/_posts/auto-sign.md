title: "Python+SAE实现自动签到"
date: 2015-05-10 20:39:03
tags:
- "寂寞的周末"
- "python"
---
## 0x00.起源
最近某偶然会上的网站启用了签到机制，需要签到满xx天才能升级，一天不签直接打回原点，而像我这么懒的人摆明了不可能每天去点那一下，就捣鼓了一个自动签到的程序
<!-- more -->
## 0x01.签到
程序逻辑十分简单，基本属于看着就能懂的

````
#!/usr/bin/env python
# -*- coding: utf-8 -*-
  
import HTMLParser  
import urlparse  
import urllib  
import urllib2  
import cookielib  
import string  
import re
import time
import logging
#登录的主页面  
loginurl = ''  #login page url
loginpostrl = '' #login post url
signurl = '' #sign page url
signposturl = '' ##sign post url
username = '' 
password = ''
remember = '1'
loginResult = []
#设置一个cookie处理器，它负责从服务器下载cookie到本地，并且在发送请求时带上本地的cookie  
cj = cookielib.LWPCookieJar()  
cookie_support = urllib2.HTTPCookieProcessor(cj)  
opener = urllib2.build_opener(cookie_support, urllib2.HTTPHandler)  
urllib2.install_opener(opener)  
  
#打开登录主页面（他的目的是从页面下载cookie，这样我们在再送post数据时就有cookie了，否则发送不成功）  
h = urllib2.urlopen(loginurl)  
  
#构造header，一般header至少要包含一下两项。这两项是从抓到的包里分析得出的。  
headers = {'User-Agent' : 'Mozilla/5.0 (Windows NT 6.1; WOW64; rv:14.0) Gecko/20100101 Firefox/14.0.1',  
           'Referer' : ''}

#构造Post数据，他也是从抓大的包里分析得出的。  
postData = {'account' : username,  
            'password' : password,  
            'remember' : remember,    
            'url_back' : ''   
            }
  
#需要给Post数据编码  
postData = urllib.urlencode(postData)  
request = urllib2.Request(loginpostrl, postData, headers) 
response = urllib2.urlopen(request)  
text = response.read()
print text
loginResult.append(text)
urllib2.urlopen(signurl)
#等待15秒后发起签到
time.sleep(15)
request2 = urllib2.Request(signposturl,headers=headers)
response2 = urllib2.urlopen(request2)
text = response2.read()
print text
loginResult.append(text)



def application(environ, start_response):
    start_response('200 ok', [('content-type', 'text/plain')])
    return ""

````

## 0x02.自动执行
上面的程序只有自动发起签到的功能，但如果每天要手动执行一次的话，那和手动签到也没什么区别了，所以需要找个能自动执行程序的平台  
恰巧听说新浪的sae开始免费了，就找来一试

1. 首先登录[新浪sae首页](http://sae.sina.com.cn/)，直接用微博帐号就能登录，然后新建一个应用，并把刚才写的代码上传  
![sae应用页面](/css/images/auto-sign/1.png "sae应用页面")
2. 配置cron自动执行，语法类似linux系统的crontab配置  
![cron配置](/css/images/auto-sign/2.png "cron配置")

每次执行后程序print所打印的日志可以在sae的日志中心的debug分类里直接查看  
![执行日志](/css/images/auto-sign/3.png "执行日志")
