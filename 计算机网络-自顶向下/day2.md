# 应用层

> 主机程序的体系结构

* 服务器-客户端体系结构
* P2P体系结构(对等体系结构)

> 嵌套字(socket)

是同一台主机内应用层和传输层之间的接口，由于该嵌套字是建立网络应用程序的可编辑接口，所以嵌套字也称为应用程序与网络之间的**应用程序编辑接口**

在因特网中**IP地址**作为主机标识符，除了需要知道目的地的主机地址外，我们还需要明确接收主机上的接收进程，这个我们用**端口号**来实现

嵌套字是应用程序进程与运输层之间的接口。当开发应用程序的时候，需要选择一种可用的运输层协议，主要参考如下：**可靠数据传输**，**吞吐量**，**定时**，**安全性**

* 可靠数据传输

确保发送端数据正确的，完整的交付给接收方

* 吞吐量

该可用的传输速率

运输层提供确保可用的吞吐量

对吞吐量有确定要求的应用程序叫**带宽敏感应用**，而没要求的是**弹性应用**

* 定时

对时延有一定保证

* 安全性

提供加密，数据完整性，端点鉴别等安全性功能

TCP可以提供可靠数据传输，SSL可以提供安全性，但是当今网络运输层还没提供确定定时时间和吞吐量服务，但是否时间敏感应用不能在当今网络中使用呢，答案肯定是否定的。好的设计其实可以做到很好的时延和带宽限制，所以，今天因特网能够为时间带宽敏感的应用提供满意的服务，但不能提供定时和吞吐量保证

---------

应用层可以选择运输层协议TCP或UDP

#### TCP

提供**面向连接服务**和**可靠数据传输服务**

> 面向连接服务

就是一个握手过程，提供在客户端和服务端交换传输层控制信息，在握手之后，在连个进程嵌套字之间建立一个TCP连接，这个连接是**双工**的

> 可靠数据传输服务

进程依靠TCP无差错，按适当顺序交付所有发送数据。当应用程序的字节流传入嵌套字时，它能依靠TCP将相同的字节流交付给接收方的嵌套字，且无丢失和冗余

TCP还提供拥塞控制机制。当网络出现拥塞时，TCP会抑制发送进程。这个对整体网络有好处

> SSL(Secure Sockets Layer)安全嵌套字层

无论是TCP还是UDP都不提供加密服务，为了安全考虑，研制了TCP的加强版本SSL，SSL是对TCP的加强，其本身不是一种协议，该加强应用于应用层。如果应用层使用了SSL，那应用层将明文数据传入SSL嵌套字，经SSL加密，传入TCP嵌套字，然后经网络传输到接收放TCP嵌套字。接收方TCP嵌套字再传入接收方SSL解密，然后传入应用层

#### UDP

它是无连接，不提供可靠数据传输和不提供拥塞控制，仅提供最小服务


----------

#### 应用层协议

* 定义了报文类型，例如请求报文和响应报文
* 各种报文的语法，报文中的各个字段和这些字段的描述
* 字段的语义，即字段包含的信息含义
* 进程如何和何时发送报文，和对报文响应规则

#### HTTP

HTTP使用TCP作为支撑运输协议

HTTP不保存客户的任何信息，所以我们说HTTP是**无状态协议**

HTTP同时支持**持续连接**和**非持续连接**,默认持续连接。持续连接就是公用一个TCP连接，非持续连接是每个HTTP连接都重新创建一个TCP连接，两个方式各有优劣，非持续连接每次HTTP请求都需要TCP三次握手，一次请求需要两次往返时间RTT(即从客户端发送请求到服务端响应，然后客户端接收响应的时间)

![图片](https://github.com/modonglinaguadu/study-note/blob/master/image/network/RTT.png)

持续连接中，一条连接如果间隔一段时间仍未使用(可以设置间隔时间)，HTTP服务器会关闭连接

#### HTTP报文格式

> 请求报文格式

![图片](https://github.com/modonglinaguadu/study-note/blob/master/image/network/http_request.png)

这个是典型的HTTP请求报文

第一行是**请求行**，后面的行是**请求头**(首部行)

**请求行**

请求行包含了方法字段，URL字段和HTTP版本字段

**请求头(首部行)**

* Host 指定主机
* Connection keep-alive告诉服务器保持持续连接，close是非持续连接
* Refere 来源网址，即从哪个网址来到这个网页


下图是HTTP请求报文结构，除了请求行和请求头，还有一个请求体,get请求时，请求体为空
![图片](https://github.com/modonglinaguadu/study-note/blob/master/image/network/http_request_post.png)
![图片](https://github.com/modonglinaguadu/study-note/blob/master/image/network/http_request1.png)

> 响应报文格式

![图片](https://github.com/modonglinaguadu/study-note/blob/master/image/network/http_response1.png)

响应报文也是包含三个部分，**状态行**，**请求头**，**请求体**

![图片](https://github.com/modonglinaguadu/study-note/blob/master/image/network/http_response.png)

> cookie

cookie技术有4个组件

1. 响应报文中请求头中一个cookie行
2. 请求报文中请求头中一个cookie行
3. 用户系统中保存一份cookie文件，并且由浏览器管理
4. 位于web站点的一个后端数据库

#### web缓存

web缓存器也叫代理服务器

![图片](https://github.com/modonglinaguadu/study-note/blob/master/image/network/http_proxy.png)

> 工作流程

web缓存服务器即是客户也是服务器，当客户端向代理服务器发出http请求，代理服务器会查看是否有请求的资源缓存，如果有即直接响应返回缓存的资源，如果没有，向源服务器发起http请求，请求客户端需要的资源，等拿到资源后，把资源存储在代理服务器，并把资源返回客户端。

> 使用代理服务器原因

1. 大大减少客户请求响应时间，特别是当客户端到代理服务器之间瓶颈带宽大于客户端到源服务器之间的瓶颈带宽时
2. web缓存器可以大大减少一个机构(比如校园网络，校园网络中可以添加一组代理服务器，然后设置校园网的客户端指向代理服务器)的接入链路到因特网的通信量，通过减少通信量，机构就不用急于增加带宽，减少费用
3. web缓存能降低因特网上的web流量，从而改善所有应用性能

> CDN(内容分发网络)

CDN公司在因特网上安装了许多地理上分散的缓存服务器，因而使大量流量本地化

> 缓存版本问题

缓存服务器虽然提供了很多方便，但是也产生一个新的问题，那就是缓存的版本是旧版本

> 条件GET请求

当HTTP请求包含if-xxx这个的首部时，服务器会对附带的条件进行判断，只有判断条件为真，才会执行请求，这样的请求首部有5个

* If-Modified-Since
* If-Unmodified-Since
* If-Match
* If-None-Match
* If-Range

这里解决缓存版本问题，我们使用了If-Modified-Since来解决

1. 缓存服务器在向web服务器请求资源时，web服务器会在响应报文中添加**Last-Modified**首部，该首部值是该资源修改的时间
2. 下一次，缓存服务器再向web服务器发送资源请求时，会在请求报文中添加**If-Modified-Since**首部，该首部值是上次web服务器响应报文中**Last-Modified**的值
3. web服务器会根据请求报文中**If-Modified-Since**的值判断资源是否更新，如果没有更新，就返回304状态，就是未更新，如果更新了，就返回新的资源，并在响应报文中添加新的**Last-Modified**首部

本地浏览器也有缓存功能和上面缓存服务器的工作原理是一样的

资源有新版本的响应报文：
``` 
HTTP/1.1 200 OK
Date: Tue, 08 Sep 2015 06:43:02 GMT
Content-Type: application/javascript
Content-Length: 94020
Connection: keep-alive
Vary: Accept-Encoding
Cache-Control: public,max-age=25920000
Last-Modified: Fri, 15 Feb 2013 03:06:57 GMT
Accept-Ranges: bytes
ETag: "7468b58329bce1:0"
```

资源没有更新的响应报文
``` 
HTTP/1.1 304 Not Modified
Date: Tue, 08 Sep 2015 06:38:40 GMT
Connection: keep-alive
Cache-Control: public,max-age=25920000
Accept-Ranges: bytes
ETag: "7468b58329bce1:0"
```


#### FTP

FTP和HTTP一样，都是文件传输协议，都使用了TCP协议，然而两个协议之间有很多不同，最大的不同是FTP使用了两个并行的TCP连接来传输文件，一个是**控制连接**，一个是**数据连接**

* 控制连接用于两个主机之间的传输控制信息，比如用户标识，口令，改变远程目录的命令，存放和获取文件等命令
* 数据连接用于实际传输文件

![图片](https://github.com/modonglinaguadu/study-note/blob/master/image/network/ftp.png)