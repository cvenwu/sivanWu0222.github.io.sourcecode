---
title: '[Python 爬虫篇](2)爬取网页遇到403错误 模拟浏览器进行爬取'
tags:
  - Python
  - 爬虫
  - 精通Python网络爬虫、核心技术、框架与项目实战
share: true
categories:
  - Python
  - 爬虫
reward: true
comment: true
top: 2
repo: sivanWu0222 | MasterPythonWebCrawler
date: 2019-04-27 16:44:40
description:
---

> 出现403错误，是因为浏览器的这些网页为了防止我们恶意采集数据而进行的反爬虫设置，我们需要修改报头来模拟浏览器

通过在头部设置User-Agent信息来模拟浏览器

## 使用build_opener()修改报头

格式：
1. headers = ("User-Agent", 具体信息)
2. opener对象名.addheaders = [头信息]

```python
import urllib.request

# url = "http://blog.csdn.com/weiwei_pig/article/details/51178226"
# 定义要爬取的地址
url = "http://www.baidu.com"
file = urllib.request.urlopen(url).read()
print(file)
# 定义一个变量存储对应的Users-Agent信息，格式为("User-Agent", 具体信息)
headers = ("User-Agent", "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.103 Safari/537.36")
# 创建自定义的opener对象并赋值给变量opener
opener = urllib.request.build_opener()
# 设置opener对象的addheaders，即设置对应的头信息
opener.addheaders = [headers]
# 设置好头部信息，就可以使用opener对象的open()方法打开对应的网址
# 此时，将会采用刚才添加的头部信息模拟浏览器打开网站
data = opener.open(url).read()
print(data)
```

## 使用add_header()修改报头
> 可以使用urllib.request.Request下的add_header()方法来修改报头
> 

**格式为 req对象.add_header(字段名，字段值)**

```python
import urllib.request
# 1. 首先设置要爬取的网址
url = "http://www.baidu.com"
# 2. urllib.request.Request(url)创建一个Request对象并且赋值给变量req
req = urllib.request.Request(url)
# 3. req对象使用add_header()方法添加对应的报头信息，格式为 req对象.add_header(字段名，字段值)
req.add_header("User-Agent", "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.103 Safari/537.36")
# 4. 使用设置好的req对象打开url链接，此时将会打开并读取网页内容赋值给data变量
data = urllib.request.urlopen(req).read()
print(data)
```

<!-- more -->