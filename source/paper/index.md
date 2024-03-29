---
title: paper
date: 2019-11-11 12:23:20
---

# python使用线程池进行编程
import concurrent
from concurrent import futures  # 做多线程和多进程编程
from concurrent.futures import ThreadPoolExecutor
from concurrent.futures import as_completed

# 线程池
## 为什么需要线程池
## 主线程中可以获取某一个线程的状态或者某一个任务的状态以及返回值
## 当一个线程完成的时候我们主线程也可以立即知道
## futures可以让多线程和多进程编码一致

import time
def get_html(times):
    time.sleep(times)  # 休眠

    print('get page {} success'.format(times))
    return times



def main():
    executor = ThreadPoolExecutor(max_workers=2)

    # 提交任务
    ## 通过submit函数提交执行的函数到线程池中
    ## submit是非阻塞的，将会立即返回
    task1 = executor.submit(get_html, (3))  # 第一个参数是函数名称，第二个参数是函数的参数
    task2 = executor.submit(get_html, (10))  # 会有一个返回结果

    # done 方法用于判定某个任务是否完成
    print(task1.done())  # 判断这个函数是否执行成功
    time.sleep(4)
    print(task2.done())

    # result 方法得到task执行的返回结果
    print(task2.result())
    print(task1.result())

    # 将某一个任务cancel掉， 在submit返回的对象上进行操作，
    task2.cancel()  # 只有在还没有执行的时候cancel掉


# 获取已经成功的task的返回as_completed 是一个生成器
executor = ThreadPoolExecutor(max_workers=4)
urls = [10, 10, 10]
all_tasks = [executor.submit(get_html, url) for url in urls]

# 异步运行
# for future in as_completed(all_tasks):  # 返回已经完成的future
    # data = future.result()  # 得到执行的结果
    # print(data)

# 通过executor获取已经完成的task， 会按照urls中的元素的顺序执行，但也是并发只是会等待
# for data in executor.map(get_html, urls):  # 直接返回执行的结果
#     print(data)



## json使用

> json.loads() 参数是一个str,可以将str转换为dict
> json.load() 参数是一个文件对象，可以将文件句柄转换为一个dict

```python
import json
import requests
url = 'http://www.baidu.com'
json.loads(requests.get(url).text)
json.load(open('1.file', 'r'))
```


## Examples 多线程爬取示范代码
```python

import concurrent
from concurrent.futures import ThreadPoolExecutor
from urllib import request


class TestThreadPoolExecutor(object):
    def __init__(self):
        self.urls = [
            'https://www.baidu.com/',
            'http://www.sina.com/',
            'http://www.csdn.net/',
            'https://juejin.im/',
            'https://www.zhihu.com/'
        ]

    def get_web_content(self, url=None):
        print('start get web content from: '+url)
        try:
            headers = {"User-Agent": "Mozilla/5.0 (X11; Linux x86_64)"}
            req = request.Request(url, headers=headers)
            return request.urlopen(req).read().decode("utf-8")
        except BaseException as e:
            print(str(e))
            return None

    def runner(self):
        thread_pool = ThreadPoolExecutor(max_workers=6)
        futures = dict()
        for url in self.urls:
            future = thread_pool.submit(self.get_web_content, url)
            futures[future] = url

        for future in concurrent.futures.as_completed(futures):
            url = futures[future]
            try:
                data = future.result()
            except Exception as e:
                print('Run thread url ('+url+') error. '+str(e))
            else:
                print(url+'Request data ok. size='+str(len(data)))
        print('Finished!')


if __name__ == '__main__':
    TestThreadPoolExecutor().runner()

```

```python
from concurrent.futures import ThreadPoolExecutor
import concurrent
import requests
urls = [
            'https://www.baidu.com/',
            'http://www.sina.com/',
            'http://www.csdn.net/',
            'https://juejin.im/',
            'https://www.zhihu.com/'
]

def sppider(url):
    print('开始爬取：', url)
    return requests.get(url).text


def run():
    thread_pool = ThreadPoolExecutor(max_workers=4)
    futures = dict()
    for url in urls:
        future = thread_pool.submit(sppider, url)
        print(future)
        print(type(future))
        futures[future] = url

    for future in concurrent.futures.as_completed(futures):
        url = futures[future]
        try:
            data = future.result()
            print(type(data))
            print(data)
        except Exception as e:
            print('Run thread url (' + url + ') error. ' + str(e))
        else:
            print(url + 'Request data ok. size=' + str(len(data)))
    print('Finished!')



if __name__ == '__main__':
    run()

```

## csv 库的使用

### 写csv文件

```python
import csv
if __name__ == '__main__':
    with open('result.csv', "a+", newline='') as f:
        writer = csv.writer(f, delimiter=':')  # delimiter用于csv文件中分隔符的显示,默认是,
        writer.writerow(['1', '2', 3])
```





## 使用os内置库创建目录


```python

import argparse
import os

# 创建目录存放生成的图片
## os.makedirs 会递归创建目录
## os.mkdir 只会创建子目录
os.makedirs('images', exist_ok=True)  # 如果exist_ok是False,目标目录存在时返回OSError
os.mkdir('path') 

```

## 从一个对象知道对应的类的名字

```python

def class_name(cl):
    # return cl.__class__  # <class '__main__.config'>
    return cl.__class__.__name__  # 返回类名字 config

class config:
    def __init__(self):
        pass


if __name__ == '__main__':
    con = config()
    print(class_name(con))
```

## 
np.prod() 默认计算所有元素乘积，无论传入进来的是一个向量还是一个tensor, 可以指定axis=1用来获取每一行的乘积




## Conclusion
1. Finally, **so far** transfer learning **techniques** have been mainly applied to **small scale applications** with **a limited variety**
2. 
