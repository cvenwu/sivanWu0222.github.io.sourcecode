---
title: opencv-视频篇
tags:
  - OpenCV
  - Python
share: true
categories:
  - OpenCV
reward: true
comment: true
top: 2
repo: 'sivanWu0222 | sivanWu0222 | OpenCV_Python'
date: 2020-01-12 10:54:19
description:
---

## 用摄像头捕获视频
使用函数cv2.VideoCapture()，其中里面需要传入一个参数0表示笔记本内置摄像头，可以设置1或别的数字来设置别的摄像头


注意：
1. cap.read() 返回两个值：第一个值：一个布尔值（True/False）。 如果帧读取的是正确的， 就是 True。所以最后你可以通过检查他的返回值来查看视频文件是否已经到了结尾。第二个值：读取到的视频帧

2. 有时 cap 可能不能成功的初始化摄像头设备。这种情况下上面的代码会报 错。你可以使用 cap.isOpened()，来检查是否成功初始化了。如果返回值是 True，那就没有问题。否则就要使用函数 cap.open()
3. 可以使用函数 cap.get(propId) 来获得视频的一些参数信息。 这里 propId 可以是 0 到 18 之间的任何整数。每一个数代表视频的一个属性，见 下表

{% image cap属性列表.png  糟了 cap属性列表%}

<!--more-->

例子：摄像头捕获视频，并将其转换为灰度图显示
```python
"""
@File : 摄像头捕获视频.py

@Author: sivan Wu

@Date : 2020/1/11 12:03 下午

@Desc : 为了获取视频，你应该创建一个 VideoCapture 对象
他的参数可以是 设备的索引号，或者是一个视频文件。设备索引号就是在指定要使用的摄像头。 一般的笔记本电脑都有内置摄像头。所以参数就是 0。
你可以通过设置成 1 或 者其他的来选择别的摄像头。之后，你就可以一帧一帧的捕获视频了。但是最 后，别忘了停止捕获视频
"""

import cv2

cap = cv2.VideoCapture(0)

# while True:
while cap.isOpened():  # 检查是否成功初始化摄像头, 如果返回值为True,就没有问题了，否则就需要使用函数cap.open()
    # Capture frame-by-frame
    # cap.read() 返回一个布尔值（True/False）。 如果帧读取的是正确的，就是True。
    # 所以最后你可以通过检查他的返回值来查看视频文件是否已经到了结尾
    ret, frame = cap.read()
    # Our operations on the frame come here
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)


    # 获得额外的视频信息
    ## 使用函数 cap.get(propId) 来获得视频的一些参数信息, propId 可以是 0 到 18 之间的任何整数。每一个数代表视频的一个属性
    ## 其中的一些值可以使用 cap.set(propId,value) 来修改，value 就是 你想要设置成的新值
    width = cap.get(3)
    height = cap.get(4)
    print(width, height)

    # Display the resulting frame
    cv2.imshow("frame", gray)
    # 0xFF：0xFF是一个位掩码，十六进制常数，二进制值为11111111, 它将左边的24位设置为0,
    # 把返回值限制在在0和255之间。ord(’ ')返回按键对应的整数（ASCII码）
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

# When everything done, release the capture
cap.release()
cv2.destroyAllWindows()
```


## 从文件中读取视频

> 只需要将函数cv2.VideoCapture()里面的参数修改成文件所在的路径以及文件名

```python
"""
@File : 从文件中播放视频.py

@Author: sivan Wu

@Date : 2020/1/11 3:45 下午

@Desc :
            与从摄像头中捕获一样，你只需要把设备索引号改成视频文件的名字。在播放每一帧时，使用 cv2.waiKey() 设置适当的持续时间。
            如果设置的太低视 频就会播放的非常快，如果设置的太高就会播放的很慢（你可以使用这种方法 控制视频的播放速度）。
            通常情况下 25 毫秒就可以了
"""


import cv2
import numpy as np

cap = cv2.VideoCapture("./自己写代码/10秒.mp4")

while cap.isOpened():
    ret, frame = cap.read()
    if ret == True:
        cv2.imshow("frame", frame)
        if cv2.waitKey(25) & 0xFF == ord('q'):
            break
    else:
        break
cap.release()
cv2.destroyAllWindows()
```

## 保存视频
> 使用VideoWriter(filename, fourcc, fps, frameSize[, isColor])函数进行写入视频

编码支持列表：[参考](http://www.fourcc.org/codecs.php#letter_i)

**VideoWriter(filename, fourcc, fps, frameSize[, isColor]) -> <VideoWriter object>**

参数讲解：
1. 第一个参数是要保存的文件的路径
2. fourcc 指定编码器: 使用cv2.VideoWriter_fourcc(*'I420')创建的编码，可用编码列表参考 http://www.fourcc.org/codecs.php#letter_i
3. fps 要保存的视频的帧率
4. frameSize 要保存的文件的画面尺寸
5. isColor 指示是黑白画面还是彩色的画面


步骤：
1. 设置视频编码：fourcc = cv2.VideoWriter_fourcc(*'MJPG')
2. VideoWriter(filename, fourcc, fps, frameSize[, isColor]) 函数建立一个writer
3. 读取视频的每一帧，将每一帧使用writer输出到文件中,writer.write(frame)
4. 循环3直到读取到视频的结尾：采用ret == True进行判断，因为如果到视频结束将会返回False

```python
"""
@File : 写入到视频文件中.py

@Author: sivan Wu

@Date : 2020/1/12 10:20 上午

@Desc :

"""

import cv2

# 下面的编码采用I420, MJPG都可以
# fourcc = cv2.VideoWriter_fourcc(*'I420')
fourcc = cv2.VideoWriter_fourcc(*'MJPG')

cap = cv2.VideoCapture(0)
frame_width = int(cap.get(3))  # 获取宽度
frame_height = int(cap.get(4))  # 获取高度
# frame_width, frame_height 不能修改为其他值
writer = cv2.VideoWriter("out.avi", fourcc, 20.0, (frame_width, frame_height))
while cap.isOpened():
    ret, frame = cap.read()
    if ret == True:
        frame = cv2.flip(frame, 0)
        writer.write(frame)
        cv2.imshow("frame", frame)
        if cv2.waitKey(1) & 0xFF == ord('q'):
            break
    else:
        break

cap.release()

writer.release()
cv2.destroyAllWindows()
```


## 总结

> frame = cv2.flip(frame, 0)

摄像头捕获视频之后沿X轴反转之后保存到文件中：第一个参数表示要旋转的视频，第二个参数表示旋转的方向，0表示绕x轴旋转，大于0的数表示绕y轴旋转，小于0的负数表示绕x和y轴旋转
        

```python
"""
@File : 保存视频.py

@Author: sivan Wu

@Date : 2020/1/11 9:50 下午

@Desc : 捕获视频，并对每一帧都进行加工之后我们想要保存这个视频
        下面的代码是从摄像头中捕获视频，沿水平方向旋转每一帧并保存它。
"""

import cv2

cap = cv2.VideoCapture(0)

# Define the codec and create VideoWriter object
fourcc = cv2.VideoWriter_fourcc(*'XVID')
out = cv2.VideoWriter("output.avi", fourcc, 20.0, (60.0, 48.0))

while cap.isOpened():
    ret, frame = cap.read()
    if ret == True:
        # 第一个参数表示要旋转的视频，第二个参数表示旋转的方向，0表示绕x轴旋转，大于0的数表示绕y轴旋转，小于0的负数表示绕x和y轴旋转
        frame = cv2.flip(frame, 0)
        out.write(frame)
        cv2.imshow('frame', frame)
        if cv2.waitKey(1) & 0xFF == ord('q'):
            break
    else:
        break
cap.release()
cv2.destroyAllWindows()
```

## 参考
[learnopencv](https://github.com/spmallick/learnopencv/blob/master/VideoReadWriteDisplay/videoWrite.py)
[cv2.VideoWriter()](https://blog.csdn.net/weixin_36670529/article/details/100977537)
[OpenCV—Python视频的读取及保存
](https://blog.csdn.net/wsp_1138886114/article/details/84798977)