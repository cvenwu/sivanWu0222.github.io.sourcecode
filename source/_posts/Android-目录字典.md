---
title: '[Android]-目录字典'
date: 2016-12-25  10:52:50
tags: Android
categories: Android
photos:
- https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1491630411728&di=edb400f4407b30358789ccc025b966ee&imgtype=0&src=http%3A%2F%2Fimg0.pconline.com.cn%2Fpconline%2F1501%2F13%2F6006483_1_thumb.jpg
---



## src

存放Android应用程序源代码的文件，通过在该文件夹下，通常创建包来进行区分不同的功能，在相应的包写上对应功能的程序代码，比如将服务，广播，还有活动区别分来

<!--more-->

----------


## gen

gen文件夹是由应用程序自动生成的配置文件，不需要去手动修改

gen文件夹下面的R.java文件是该Android 应用程序资源管理文件，也就是通俗的字典，该文件将所有资源生成相对应的唯一的ID，可以通过ID对与其对应的资源进行引用

说到R.java 有时候可能无法正常生成R.java 自已有过亲身经历，自从修改了ADT之后，问题就消失了


----------

## assets:

该文件会放一些资源文件，例如多媒体文件

下面讲到的res文件夹也可以存放资源, 不过assets 与 res有一些实质性的区别：

assests下的资源文件不会自动生成资源ID 和占用apk(Android发布的可执行文件) 空间

assests文件可以通过AssetsManager类进行访问，存放到这里的资源在运行打包的时候都会打入程序安装包中


----------

## bin:

存放编译后的文件，例如 apk,dex(Android上可执行文件的类型),class,资源文件等


----------


## libs:

存放第三方使用到的JAVA架包


----------


## res:

存放各种类型的资源文件，包含不同的目录
drawble:存放图片文件，但是由于设备的不同，需要存放不同分辨率下的图片，ldpi–>mdpi–>hdpi–>xhdpi–>xxhdpi 分辨率由低到高
layout:存放布局问文件
menu:存放菜单文件
values：
dimens:存放尺寸大小
strings:存放字符串资源
styles:存放主题或者样式
colors:存放颜色


----------


## AndroidManifest.xml

这是android项目的系统清单文件，也是整个android应用的全局描述文件。清单文件说明了android应用的名称、所使用的图标以及包含的组件等，


----------


## proguard-project.txt:

代码混淆相关文件


----------


## project.properties：

工程属性的配置文件，配置编译的版本等

