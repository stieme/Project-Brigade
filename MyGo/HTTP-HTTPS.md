### 一.HTTP/HTTPS简述：

#### 1.HTTP基础：

<mark>：HTTP（超文本传输协议，Hypertext Transfer Protocol）是一种用于从网络传输超文本到本地浏览器的传输协议。它定义了客户端与服务器之间请求和响应的格式。HTTP 工作在 TCP/IP 模型之上，通常使用端口 80。</mark>



#### 2.HTTP工作原理：

<mark>：HTTP 协议工作于客户端-服务端架构上。</mark>

***HTTP 工作过程通常如下：***

1. **客户端发起请求**：用户通过客户端（如浏览器）输入 URL，客户端向服务器发起一个 HTTP 请求。
2. **服务器处理请求**：服务器接收到请求后，根据请求的类型（如GET、POST等）和请求的资源，进行相应的处理。
3. **服务器返回响应**：服务器将处理结果包装成HTTP响应消息，发送回客户端。
4. **客户端渲染页面**：客户端接收到响应后，根据响应内容（如HTML、图片等）渲染页面，展示给用户。

<mark>：HTTP 默认端口号为 80，但是也可以改为 8080 或者其他端口。</mark>

**HTTP 三点注意事项：**

* HTTP 是无连接：无连接的含义是限制每次连接只处理一个请求，服务器处理完客户的请求，并收到客户的应答后，即断开连接，采用这种方式可以节省传输时间。

* HTTP 是媒体独立的：这意味着，只要客户端和服务器知道如何处理的数据内容，任何类型的数据都可以通过HTTP发送，客户端以及服务器指定使用适合的 MIME-type 内容类型。

* HTTP 是无状态：HTTP 协议是无状态协议，无状态是指协议对于事务处理没有记忆能力，缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大，另一方面，在服务器不需要先前信息时它的应答就较快。



#### 3.HTTPS作用：

**HTTPS 的主要作用是在不安全的网络上创建一个安全信道，并可在使用适当的加密包和服务器证书可被验证且可被信任时，对窃听和中间人攻击提供合理的防护。**

**HTTPS 的信任基于预先安装在操作系统中的证书颁发机构（CA）。**

因此，与一个网站之间的 HTTPS 连线仅在这些情况下可被信任：

* 浏览器正确地实现了 HTTPS 且操作系统中安装了正确且受信任的证书颁发机构；
* 证书颁发机构仅信任合法的网站；
* 被访问的网站提供了一个有效的证书，也就是说它是一个由操作系统信任的证书颁发机构签发的（大部分浏览器会对无效的证书发出警告）；
* 该证书正确地验证了被访问的网站（例如，访问 [https://www.runoob.com](https://www.runoob.com/) 时收到了签发给 www.runoob.com 而不是其它域名的证书）；
* 此协议的加密层（SSL/TLS）能够有效地提供认证和高强度的加密。

Google Chrome、Internet Explorer 和 Firefox 等浏览器在网站含有由加密和未加密内容组成的混合内容时，会发出警告。



#### 4.HTTP/HTTPS区别：

* HTTP 明文传输，数据都是未加密的，安全性较差，HTTPS（SSL+HTTP） 数据传输过程是加密的，安全性较好。
* 使用 HTTPS 协议需要到 CA（Certificate Authority，数字证书认证机构） 申请证书，一般免费证书较少，因而需要一定费用。证书颁发机构如：Symantec、Comodo、GoDaddy 和 GlobalSign 等。
* HTTP 页面响应速度比 HTTPS 快，主要是因为 HTTP 使用 TCP 三次握手建立连接，客户端和服务器需要交换 3 个包，而 HTTPS除了 TCP 的三个包，还要加上 ssl 握手需要的 9 个包，所以一共是 12 个包。
* http 和 https 使用的是完全不同的连接方式，用的端口也不一样，前者是 80，后者是 443。
* HTTPS 其实就是建构在 SSL/TLS 之上的 HTTP 协议，所以，要比较 HTTPS 比 HTTP 要更耗费服务器资源。

![dffdff](https://www.runoob.com/wp-content/uploads/2018/09/dffdff.png)



![HTTPS 加密、解密、验证及数据传输过程.png](https://segmentfault.com/img/bVbClUl)

<mark>PS:有关SSL与TLS,不适宜在此处讲，如对工作原理等感兴趣，移步：[HTTPS详解二：SSL / TLS 工作原理和详细握手过程 - 个人文章 - SegmentFault 思否](https://segmentfault.com/a/1190000021559557)</mark>

![SSL : TLS 握手过程](https://segmentfault.com/img/bVbCCMD)





### 二.HTTP信息结构：

#### 1.HTTP信息结构：

HTTP 是基于客户端/服务端（C/S）的架构模型，通过一个可靠的链接来交换信息，是一个无状态的请求/响应协议。

HTTP 消息是客户端和服务器之间通信的基础，它们由一系列的文本行组成，遵循特定的格式和结构。

HTTP消息分为两种类型：请求消息和响应消息。（Request  Response）

一个 HTTP 客户端是一个应用程序（Web 浏览器或其他任何客户端），通过连接到服务器达到向服务器发送一个或多个 HTTP 的请求的目的。

一个 HTTP 服务器 同样也是一个应用程序（通常是一个 Web 服务，如 Nginx、Apache 服务器或 IIS 服务器等），通过接收客户端的请求并向客户端发送 HTTP 响应数据。

![231-O-Que-E-Request-E-Response-02](https://www.runoob.com/wp-content/uploads/2013/11/231-O-Que-E-Request-E-Response-02.jpg)



#### 2.客户端请求信息:






