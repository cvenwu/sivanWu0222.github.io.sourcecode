---
title: '[Python 爬虫篇](1)快速使用urllib爬取网页'
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
date: 2019-04-27 16:47:02
description:
---

### 读取内容常见的3种方式
> 在我们使用urllib打开网页之后，要读取文件的全部内容可以有如下3种方式
1. file.read()读取网页全部内容到一个字符串中
2. file.readline()读取文件的一行内容
3. file.readlines()读取网页内容到一个列表中，**如果要读取网页全部内容，建议使用这种方式**

### 读取网页内容并且保存到本地

#### 方法一：使用文件进行操作
1. 首先 爬取一个网页并将爬取到的内容读取出来赋值给一个变量
2. 以写入的方式打开一个本地文件，命名为.html文件
3. 将1中的变量中的内容写入到2中的文件
4. 关闭该文件

```python
import urllib.request
URL = "https://www.baidu.com"
data = urllib.request.urlopen(URL).read()
with open('baidu_content.html', 'wb+') as f:
    f.write(data)
```

#### 方法二：直接使用urllib.request.urlretrieve(URL, file)

> 使用urllib.request.urlretrieve(URL, file) 直接将URL对应的网页内容保存到指定的FILE文件

```python
import urllib.request
URL = "http://www.baidu.com"
urllib.request.urlretrieve(URL, '1.html')
```

**urllib.request.urlretrieve(URL, file)将网页内容保存到本地将会产生缓存，如果希望清除缓存可以采用如下的方式**
```python
urllib.request.urlcleanup()
```

### urllib的其他常见用法

```python
>>> import urllib.request
>>> file = urllib.request.urlopen("http://www.baidu.com")
# 如果希望返回与当前环境有关的信息，可以使用info()返回
>>> file.info
<bound method HTTPResponse.info of <http.client.HTTPResponse object at 0x7fd8fb0edb00>>
# 如果希望获取当前爬取网页的状态码，可以使用getcode()
>>> file.getcode()
200
# 如果想要获取爬取网页的URL地址，可以使用geturl()实现
>>> file.geturl()
'http://www.baidu.com'
```

### URL注意事项
一般来说，URL标准只允许一部分ASCII字符，因此对于中文以及其他一些字符，将会对其进行URL编码

如果要进行URL编码可以使用 urllib.request.quote(编码内容) 来进行编码
如果要进行URL解码可以使用 urllib.request.unquote(编码内容) 来进行编码

```python
>>> file.geturl()
'http://www.baidu.com'
>>> urllib.request.quote("http://www.baidu.com")
'http%3A//www.baidu.com'
```



<!--more-->