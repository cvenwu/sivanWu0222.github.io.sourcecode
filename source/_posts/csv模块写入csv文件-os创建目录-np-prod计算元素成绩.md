---
title: csv模块写入csv文件 os创建目录 np.prod计算元素乘积
tags:
  - Python
  - csv
  - os
  - numpy
share: true
categories:
  - Python
reward: true
comment: true
top: 2
date: 2019-11-30 16:39:53
description:
---


## csv 库的使用

### 写csv文件

```python
import csv
if __name__ == '__main__':
    with open('result.csv', "a+", newline='') as f:
        writer = csv.writer(f, delimiter=':')  # delimiter用于csv文件中分隔符的显示,默认是,
        writer.writerow(['1', '2', 3])
```
 
np.prod() 默认计算所有元素乘积，无论传入进来的是一个向量还是一个tensor, 可以指定axis=1用来获取每一行的乘积

<!--more-->

### 写入和读取csv文件
```python
with open(os.path.join(self.root, filename), mode='w', newline='') as f:
                writer = csv.writer(f)
                for img in images:
                    name = img.split(os.sep)[-2]
                    label = self.name2label[name]
                    writer.writerow([img, label])
    
        imgs, labels = [], []
with open(os.path.join(self.root, filename)) as f:
    reader = csv.reader(f)
    for line in reader:
        ## line 将会被认为是一个列表，将行尾部的换行符去掉之后将剩下的内容按照,进行划分得到的列表
        img, label = line
        label = int(label)
        
        imgs.append(img)
        labels.append(label)
```

## glob库的使用
> glob根据指定文件名字的规则进行文件名的搜寻，并返回一个列表

```python
# glob模块根据规则匹配特定的文件名字并返回一个列表,所以采用+将列表进行拼接而不是作为元素加入列表中
            images += (glob.glob(os.path.join(self.root, name, '*.png')))
            images += (glob.glob(os.path.join(self.root, name, '*.jpeg')))
            images += (glob.glob(os.path.join(self.root, name, '*.jpg')))

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

## 使用np.prod()计算元素成绩


```python
import numpy as np
a = np.array([1, 2, 3, 4])
result = np.prod(a)  # 当指定axis=1,求行的乘积
```



