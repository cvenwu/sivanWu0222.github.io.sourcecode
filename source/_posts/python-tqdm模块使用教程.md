---
title: python tqdm模块使用教程
tags:
  - Python
  - tqdm
share: true
categories:
  - Python
  - tqdm
reward: true
comment: true
top: 2
date: 2019-11-15 20:48:13
description:
---

## tqdm 使用教程
> tqdm 是一个Python进度条模块，用于显示读取进度或者处理进度

```python

import tqdm
from time import sleep
# 第一种方法
for i in tqdm.tqdm(range(2)):
    print(i)
    sleep(1)
# 第二种方法
for i in tqdm.trange(2):
    print(i)
    sleep(1)

# 第三种方法
for i in tqdm.tnrange(2, desc='ours'):
    print(i)
    sleep(1)
# 第四种方法
pbar = tqdm.trange(10)
for i in pbar:
    print(i)
    pbar.set_description('kaishi')
    sleep(1)

# 运行结果如下：
'''
/Users/yirufeng/anaconda3/bin/python3.7 /Users/yirufeng/git_repository/DLNN/tqdm使用教程.py
0
 50%|█████     | 1/2 [00:01<00:01,  1.00s/it]1
100%|██████████| 2/2 [00:02<00:00,  1.00s/it]
  0%|          | 0/2 [00:00<?, ?it/s]0
 50%|█████     | 1/2 [00:01<00:01,  1.00s/it]1
100%|██████████| 2/2 [00:02<00:00,  1.00s/it]
HBox(children=(IntProgress(value=0, description='ours', max=2, style=ProgressStyle(description_width='initial')), HTML(value='')))
0
1

0
kaishi:   0%|          | 0/10 [00:00<?, ?it/s]1
kaishi:  10%|█         | 1/10 [00:01<00:09,  1.01s/it]2
kaishi:  20%|██        | 2/10 [00:02<00:08,  1.00s/it]3
kaishi:  40%|████      | 4/10 [00:04<00:06,  1.00s/it]4
5
kaishi:  50%|█████     | 5/10 [00:05<00:05,  1.00s/it]6
kaishi:  70%|███████   | 7/10 [00:07<00:03,  1.00s/it]7
8
kaishi:  80%|████████  | 8/10 [00:08<00:02,  1.00s/it]9
kaishi: 100%|██████████| 10/10 [00:10<00:00,  1.00s/it]

Process finished with exit code 0
'''

```


<!--more-->