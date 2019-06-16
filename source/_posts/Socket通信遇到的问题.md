---
title: Socket通信遇到的问题
date: 2017-05-19 10:31:36
tags: [Java, net]
categories:
- Java
- socket
---
> 最近在进行socket编程的时候，遇到一个问题，将代码粘在了下边，由于之前遇到过该问题，所以就记录下来，顺便总结一下解决思路

### 产生的异常如下
      Software caused connection abort: recv failed
      java.net.SocketException: Software caused connection abort: recv failed
      at java.net.SocketInputStream.socketRead0(Native Method)
      at java.net.SocketInputStream.read(SocketInputStream.java:129)
      .......

### 代码如下
Client类：
```Java
import java.net.*;
import java.io.*;
import java.util.*;

public class MyTalkClient {
  public static void main(String[] args){
    try{
        Socket socket = new Socket("127.0.0.1", 8080);
        while(true){
          OutputStream out = socket.getOutputStream();
          DataOutputStream dos = new DataOutputStream(out);
          Scanner input = new Scanner(System.in);
          dos.writeUTF(input.next());
          InputStream in = socket.getInputStream();
          DataInputStream dis = new DataInputStream(in);
          System.out.print("Server:");
          /*关于这里为什么会抛出一个异常*/
          System.out.println(dis.readUTF());
      }
    }catch(Exception e){
      e.printStackTrace();
      System.out.println("Error");
      System.exit(0);
    }
  }
}

```
<!-- more -->
Server类
```Java
import java.net.*;
import java.io.*;
import java.util.*;

public class MyTalkServer{
  /*自己写一个类似于talkserver talkclient的程序*/
  public static void main(String[] args){
    try{
      ServerSocket socket = new ServerSocket(8080);
      while (true){
          Socket s = socket.accept();
          InputStream in = s.getInputStream();
          DataInputStream dis = new DataInputStream(in);
          System.out.println("From:" + s.getInetAddress() + "\\" + s.getPort());
          System.out.println(dis.readUTF());
          OutputStream out = s.getOutputStream();
          DataOutputStream dos = new DataOutputStream(out);
          Scanner input = new Scanner(System.in);
          dos.writeUTF(input.next());
          dos.flush();
          dos.close();
          dis.close();
          out.close();
          in.close();
        }
      }catch(Exception e){
        e.printStackTrace();
        System.out.println("Error");
        System.exit(0);
      }
  }
}
```


----------

### 解决思路：
>1. 客户端和服务端建立tcp的短连接,每次客户端发送一次请求, 服务端响应后关闭与客户端的连接. 如果客户端在服务端关闭连接后,没有释放连接,继续试图发送请求和接收响应. 这个时候就会出错.

>2. 这个时候客户端Socket的getOutputStream返回来的OutPutStream维护 的是本地的连接状态, 无法知道远程的服务端已经关闭了对应的InputStream和socket因此 虽然调用了 out.write(sendbuf, 0, sendbuf.length); 方法,但是实际上服务端并没有接收到客户端的请求信息. 因为没有抛出异常,因此造成了误以为客户端请求发送成功的假象.

>3. 接下来调用etInputStream的in.read(header, 0, 14);方法. 因为这次要读取服务端的信息,因此产生了 Software caused connection abort: recv failed的异常

1. 在服务端/客户端单方面关闭连接的情况下,另一方依然以为 tcp连接仍然建立,试图读取对方的响应数据,导致出现 Software caused connection abort: recv failed的异常.因此在receive数据之前,要先判断连接状态. 通过inputstream的available()方法来判断,是否有响应结果.
2. 如果available()的返回值为0,说明没有响应数据,可能是对方已经断开连接, 如果available()的返回值大于0,说明有响应数据. 另外值得注意的是available()返回的值是非堵塞的,可以被多个线程访问.在对方释放连接后,也要释放本地的连接.
