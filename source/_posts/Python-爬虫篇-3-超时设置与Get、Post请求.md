---
title: '[Python 爬虫篇](3)超时设置与Get、Post请求'
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
date: 2019-04-28 10:17:13
description:
---



## 超时设置

当我们使用爬取网页时，我们可以设置超时时间，以便可以更快或者重新访问网页，从而避免在获取响应网页内容上浪费太多时间



语法如下：
> urllib.request.urlopen(timeout=1) # 设置1秒为超时界限

```python
import urllib.request
url = "http://www.google.com.hk"
for i in range(1, 100):
    try:
        # 超时设置为1秒钟，即1秒钟未响应则判定超时，并读取该网站的内容，输出获取到内容的长度
        file = urllib.request.urlopen(url, timeout=1)
        data = file.read()
        print(len(data))
    except Exception as e:
        print("出现异常" + str(e))
```


## 爬虫之Get请求

案例：使用get请求获取百度
