---

title: demo
date: 2019-04-16 12:47:35
---

## 图片

### 正常显示

#### 不带标题的图片

<img src="./index/img.jpg" class="normal">

- 上图为无标题的图

- 保持宽高比，缩放至与文章文字对其
- 有圆角



#### 带标题的图片

{% image ./index/img.jpg 糟了 这是标题%}   

-  有标题



### 缩放

<img src="./index/img.jpg" class="small">

{% image ./index/img.jpg 糟了 这是标题 small %}   


  <img src="./index/img.jpg" class="full">



{% image ./index/img.jpg 糟了 这是标题 full %}   

<img src="./index/img.jpg" class="justify-small">         

{% image ./index/img.jpg 糟了 这是标题 justify-small %}  

<img src="./index/img.jpg" class="justify-full">         

{% image ./index/img.jpg 糟了 这是标题 justify-full %}   



## 表格

<table class="table"> <!-- class="table"用于处理表格顶部大段空白的现象 -->
    <tr>
        <th>节次/周次</th>
        <th>星期一</th>
        <th>星期二</th>
        <th>星期三</th>
        <th>星期四</th>
        <th>星期五</th>
    </tr>
    <tr>
        <td>第一节</td>
        <td></td>
        <td></td>
        <td rowspan="2">高等数学（西205)</td>
        <td rowspan="2">高等数学（西206)</td>
        <td></td>
    </tr>
    <tr>
        <td>第二节</td>
        <td></td>
        <td rowspan="3">C++（西205）</td>
        <td></td>
    </tr>
    <tr>
        <td>第三节</td>
        <td rowspan="2">高等数学（西205)</td>
        <td rowspan="2">数据结构（西205）</td>
        <td rowspan="2">高等数学（西206）</td>
        <td rowspan="2">大学英语（至205）</td>
    </tr>
    <tr>
        <td>第四节</td>
    </tr>
    <tr>
        <td>第五节</td>
        <td rowspan="2">数据结构（西203）</td>
        <td rowspan="2">离散数学（西407）</td>
        <td rowspan="2">大学体育</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>第六节</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>第七节</td>
        <td rowspan="2">近代史（至305）</td>
        <td></td>
        <td rowspan="2">大学英语（至205）</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>第八节</td>
        <td></td>
        <td></td>
        <td></td>
    </tr>
</table>





### 引用

{% blockquote David Levithan, Wide Awake %}

Do not just seek happiness for yourself. Seek happiness for all. Through kindness. Through mercy.

{% endblockquote %}



### 布局



@column-2{



@card{



\# 左



}



@card{



\# 右



}



}

#### 布局2

@column-3{



@card{



左



}



@card{



@center{

中

}


}

@card{

@right{

右

}

}

}


### 时间轴

@timeline{

##### 2016

@item{

###### 11月6日

为 `Card theme` 添加 `page layout`。
第二行测试

}

@item{

###### 11月20日

另一个 Time line .
第二行测试

}

}
