---
title: 浅谈Java中的文件和编码
date: 2017-05-09 18:08:08
tags: [Java,File]
categories:
- Java
- File
---

>前言：许多编程语言文件操作都极其类似，Java也不尽相同，文件操作，只能获取文件（目录）的信息（名称，大小等），却不能用于文件的访问

# File类讲解

## 如何创建指定文件
> 我们可以使用提供的createNewFile()方法来创建给定文件名的文件，但是如果我们写文件名的时候没有写上后缀，那么创建的文件也是没有后缀的

假使我们创建一个 我爱学习.txt在E盘符下

```Java
public class CreateFile {
	public static void main(String[] args) {
		File file = new File("E:\\我爱学习.txt");
    /*下面这行代码也可以创建指定文件*/
  /*  File file = new File("E:","我爱学习.txt");  */
		/*首先判断文件是否存在*/
		if(!file.exists()){
			try {
				file.createNewFile();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
	}
}
```
<!--more-->
----------

## File类中的mkdir 与 mkdirs的区别
1. mkdir 只能创建一个已有目录的直接子目录
2. mkdirs 可以创建多个目录（也就是说，倘若创建目录的时候，上一级目录不存在，上一级目录也会创建）

### 使用mkdir()创建一个目录
> 如果这里《我爱学习》目录不存在，那么就不会创建《学习也爱我》的目录

```Java
public class CreateFile {

	public static void main(String[] args) {
		File file = new File("E:\\我爱学习\\学习也爱我");
		if(!file.exists()){
			System.out.println(file.mkdir());
		}
	}
}
```
### 使用mkdirs()创建一个目录
> 如果这里《我爱学习》目录不存在，那么先会自动创建《我爱学习》的目录，然后自动创建《学习也爱我》的目录
```Java
public class CreateFile {

	public static void main(String[] args) {
		File file = new File("E:\\我爱学习\\学习也爱我");
		if(!file.exists()){
			System.out.println(file.mkdir());
		}
	}
}
```
----------


## 文件目录的遍历
> 这里采用递归遍历文件和目录

```Java
public class FindFile {

	public static void main(String[] args) {

		File file = new File("E:\\navicat");
		printYourFile(file);

	}

	public static void printYourFile(File file)
	{
		File[] files = file.listFiles();
		for (File file2 : files) {
			if(file2.isFile()){
				System.out.println(file2.getAbsolutePath());
			}else{
				System.out.println(file2.getAbsolutePath());
				printYourFile(file2);
			}
		}
	}
}
```
----------
## 文件编码的问题

```Java
import java.io.UnsupportedEncodingException;

public class EncodeDemo {
  public static void main(String[] args) throws Exception {    

    String s = "简书ABC";
    byte[] bt1 = s.getBytes();
    //此处将字符串转换成字节序列用的是项目默认编码gbk
    for(byte b: bt1){
    //把字节转换成了int，以16进制显示
        System.out.print(Integer.toHexString(b&0xff)+"  ");
    //（bc  f2）简（ca  e9）书（41）A（42）B（43）C
    }


    System.out.println();
    //gbk编码，中文占用2个字节，英文占用1个字节。
    byte[] bt2 = s.getBytes("gbk");
    for(byte b: bt2){
        System.out.print(Integer.toHexString(b&0xff)+"  ");
    //（bc  f2）简（ca  e9）书（41）A（42）B（43）C
    }


    System.out.println();
    //utf-8编码，中文占用3个字节，英文占用1个字节。
    byte[] bt3 = s.getBytes("utf-8");
    for(byte b: bt3){
        System.out.print(Integer.toHexString(b&0xff)+"  ");
    //（e7  ae  80）简（e4  b9  a6）书（41）A（42）B（43）C        
    }


    System.out.println();
    //java采用双字节编码，即utf-16be编码
    byte[] bt4 = s.getBytes("utf-16be");
    //utf-16be编码，中文占用2个字节，英文占用2个字节。
    for(byte b: bt4){
        System.out.print(Integer.toHexString(b&0xff)+"  ");
    //（7b  80）简（4e  66）书（0  41）A（0  42）B（0  43）C
    }


    System.out.println();
    /*
    当字节序列使用某种编码时
    此时想把字节序列转变为字符串
    也需要使用这种编码形式
    否则会出现乱码。
    */
    String str1 = new String(bt4);//此时为项目默认编码gbk
    System.out.println(str1);//{?Nf A B C

    String str2 = new String(bt4,"utf-16be");
    System.out.println(str2);//简书ABC
    }    
}
```
