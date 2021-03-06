---
title: http和https的区别
tags:
  - 互联网
  - 面试
  - http
---

在我们上网的时候可以看到url前面会有http或者https的字样，这篇博客将解释这两个的区别。

<!--more-->

http就是我们说的超文本传输协议，这个协议是用明文的方式发送内容，比如说我们访问一个网站，可能需要
在一个网站输入密码登陆账号之类的操作，这时候使用如果有人在服务端向客户端请求数据的中途拦截了数据
是可以直接获取到账号密码信息的。

这里为了解决http在传输过程中不加密的问题，后面添加了一个ssl协议，这个协议提供了一个数据安全和完整
性的协议内容（主要负责网络连接的加密）。

访问一个https的网站请求步骤如下：

1. 客户端首先和服务端建立一个安全的连接通道。
2. 服务端发送一份网站的证书信息到客户端（告诉客户端访问到了正确的服务器）。
3. 这时候服务端将共钥，和请求发送给客户端（私钥自己保留）。
4. 客户端将数据打包加密后发送给服务端。
5. 服务器通过私钥解密数据包。
