# 图解HTTp

# 了解Web即网络基础

Web浏览器作为客户端根据地址栏的URL从服务器获取资源。Web在应用层的协议使用的是HTTP协议

## TCP/IP协议族

HTTP协议是TCP/IP协议族的内应用层的协议

TCP/IP协议内是分为四层。很明显，分层设计设计能够降低功能之间的耦合性。把各层之间的接口规划好后，每个层次内部的设计就可以只有改动

- 应用层：向上为用户提供各种各类通用的应用服务
- 传输层：对上层应用层提供处于网络连接中的两台计算机之间的数据传输服务
- 网络层：用来处理在网络上流动的数据包。该层规定了通过怎样的罗京到达对方的计算机，把数据包传给对方
- 数据链路层：用来处理连接网络的硬件部分

## URL与URL

![img](https://pic4.zhimg.com/80/f66f9f573436858aeeb2ac3da732f5a9_1440w.jpg?source=1940ef5c)

URI(统一资源标识符)：Uniform Resource Identifier

URL(统一资源定位符)：Uniform Resource Locator

URI用字符串标识某一互联网资源，而URL标识资源的地点。URL是URI的子集