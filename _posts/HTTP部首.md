---
title: HTTP部首
tags:
  - 互联网
  - HTTP
  - 面试
---

HTTP协议的请求和响应报文中必定包含HTTP Head部分。这里将介绍这个部分的内容。

HTTP协议请求和响应的报文必定包含HTTP的HEAD部分，HEAD内容为客户端和服务器分别处理请求和响应所需要提供所需要的信息。对于客户端用户来说，这些信息中的大部分内容都无须亲自查看。

<!--more-->

## 1. HTTP部首字段

HTTP报文的HEAD部分是构成HTTP报文的要素之一。在客户端与服务器之间以HTTP协议进行通信的过程中，无论是请求还是响应都会使用部首字段，他能起传递额外重要信息的作用。HEAD部分是为了服务器和浏览器之间交换报文body体大小，所使用的语言，认证信息等内容。其中部首字段的由部首字段名和字段值构成，中间用冒号 “:” 分隔。

```
部首字段名：字段值

Content-Type: text/html
Keep_Alive: timeout=15, max=100
```
> 若HTTP报文部首中出现了两个或者两个以上具有相同部首名字的字段，根据浏览器的处理逻辑确定处理方法（处理先出现的或处理后出现的）。

HTTP的部首字段根据实际用途可以分成：

* 通用部首字段，请求和响应报文两方都会使用的部首字段。
* 请求部首字段，从客户端向服务器端发送请求时使用的部首。
* 响应部首字段，从服务器端向客户端返回响应报文时使用的部首。
* 实体部首字段，针对请求报文和响应报文实体部分使用

## 2. HTTP通用部首字段

通用部首字段是指请求和响应报文双方都会使用的部首。主要包含下面字段。

| 通用字段名 | 说明 |
| :-: | :- |
| Catch-Control | 控制缓存的行为 |
| Connection | 连接管理 |
| Date | 创建报文的日期时间 |
| Pragma | 报文指令 |
| Trailer | 报文末端部首一览 |
| Transfer-Encoding | 指定报文主体的传输编码方式 |
| Upgrade | 升级为其他协议 |
| Via | 代理服务器的相关信息 |
| Warning | 错误通知 |



## 3. HTTP请求部首字段

请求部首字段是从客户端往服务器端发送请求报文中所使用的字段，用于补充请求的附加信息、客户端信息、对响应内容相关的优先级等内容。

| 部首字段名 | 说明 |
| :- | :- |
| Accept | 用户代理可处理的媒体类型 |
| Accept-Charset | 优先的字符集 |
| Accept-Encoding | 优先的内容编码 |
| Accept-Language | 优先的语言（自然语言） |
| Authorization | Web认证信息 |
| Expect | 期待服务器的特定行为 |
| From | 用户的电子邮箱地址 |
| Host | 请求资源所在的服务器 |
| If-Match | 比较实体标记（ETag） |
| If-Modified-Since | 比较资源的更新时间 |
| If-None-Match | 比较实体标记（与If-Match相反） |
| If-Range | 资源未跟新时发送实体Byte的范围请求 |
| If-Unmodified-Since | 比较资源的更新时间（与If-Modified-Since相反） |
| Max-Forwards | 最大的传输逐跳数 |
| Proxy-Authorization | 代理服务器要求客户端的认证信息 |
| Range | 实体的字节范围请求 |
| Referer | 对请求中的URI的原始获取方 |
| TE | 传输编码优先级 |
| User-Agent | HTTP客户端程序信息 |

## 4. HTTP响应部首字段

响应部首字段是由服务器端向客户端返回响应报文中使用的字段，用于补充响应的附加信息、服务器信息，以及客户端的附加要求等信息。

| 部首字段名 | 说明 |
| :- | :- |
| Accept-Ranges | 是否接收字节范围请求 |
| Age | 推算资源创建经过时间 |
| ETag | 资源的匹配信息 |
| Location | 令客户端重定位至指定URI |
| Proxy-Authorization | 代理服务器对客户端的认证信息 |
| Retry-After | 对再次发起请求的时机要求 |
| Server | HTTP服务器的安装信息 |
| Vary | 代理服务器缓存的管理信息 |
| WWW-Authenticate | 服务器对客户端的认证信息 |

## 5. HTTP实体部首字段

实体部首字段是包含在请求报文和响应报文中实体部分所使用的部首，用于补充内容更新时间等与实体内容相关的信息。

| 部首字段名 | 说明 |
| :- | :- |
| Allow | 资源可支持的HTTP方法 |
| Content-Encoding | 实体主体适用的编码方式 |
| Content-Language | 实体主体的自然语言 |
| Content-Length | 实体主体的大小（单位：字节） |
| Content-Location | 替代对应资源的URI |
| Content-MD5 | 实体主体的报文摘要 |
| Content-Range | 实体主体的位置范围 |
| Content-Type | 实体主体的媒体类型 |
| Expires | 实体主体过期的日期时间 |
| Last-Modified | 资源最后修改的日期时间 |

## 6. 为Cookie服务的其他部首字段

管理服务器与客户端之间状态的Cookie，主要用于用户识别以及状态管理。Web网站为了管理用户的状态会通过Web浏览器，将一些数据临时写入用户计算机中。接着当用户访问该Web网站时可以通过通信的方式取回之前发放的Cookie。调用Cookie时由于可以校验Cookie的有效期，以及发送方的域、路径、协议等信息，所以正规发布的Cookie内的数据不会因来自其他Web站点和攻击者的攻击而泄漏。

主要为Cookie服务的部首字段如下：

| 部首字段名称 | 说明 | 部首字段类型 |
| :- | :- | :- |
| Set-Cookie | 开始管理状态所使用的Cookie信息 | 响应部首字段 |
| Cookie | 服务器接收到的Cookie信息 | 请求部首字段 |

## 7. 其他部首字段

HTTP部首字段是可以自行扩展的。所以在Web服务器和浏览器的应用上，会出现各种非标准间的部首字段。

### 7.1 X-Frame-Options

```
X-Frame-Options: DENY
```

部首字段X-Frame-Options用于控制网站内容在其他Web网站的Frame标签内的显示问题，主要有下面两个可指定字段值：
* DENY ： 拒绝
* SAMEORIGIN ： 仅同源域名下的页面匹配时许可。

### 7.2 X-XSS-Protection

```
X-XSS-Protection: 1
```

针对跨站脚本攻击（XSS）的一种对策，用于控制浏览器的XSS防护机制开关。可以指定的字段值如下：
* 0: 将XSS过滤设置成无效状态
* 1: 将XSS过滤设置成有效状态

### 7.3 DNT

```
DNT: 1
```

表示拒绝个人信息被收集，时表示拒绝被精准广告追踪的一种方法。其中值：
* 0: 同意被追踪
* 1: 拒绝被追踪

### 7.4 P3P

主要用作让Web网站上的个人隐私变成一种仅供程序可理解的形式，达到保护用户隐私的目的。
