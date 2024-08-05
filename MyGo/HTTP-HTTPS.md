### HTTP/HTTPS简述：

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





### HTTP信息结构：

#### 1.HTTP信息结构：

HTTP 是基于客户端/服务端（C/S）的架构模型，通过一个可靠的链接来交换信息，是一个无状态的请求/响应协议。

HTTP 消息是客户端和服务器之间通信的基础，它们由一系列的文本行组成，遵循特定的格式和结构。

HTTP消息分为两种类型：请求消息和响应消息。（Request  Response）

一个 HTTP 客户端是一个应用程序（Web 浏览器或其他任何客户端），通过连接到服务器达到向服务器发送一个或多个 HTTP 的请求的目的。

一个 HTTP 服务器 同样也是一个应用程序（通常是一个 Web 服务，如 Nginx、Apache 服务器或 IIS 服务器等），通过接收客户端的请求并向客户端发送 HTTP 响应数据。

![231-O-Que-E-Request-E-Response-02](https://www.runoob.com/wp-content/uploads/2013/11/231-O-Que-E-Request-E-Response-02.jpg)



##### 1.客户端请求信息:

<mark>请求报文的一般格式:</mark>

![2012072810301161](https://www.runoob.com/wp-content/uploads/2013/11/2012072810301161.png)

* **请求行**（Request Line）：
  
  * **方法**：如 GET、POST、PUT、DELETE等，指定要执行的操作。
  * **请求 URI**（统一资源标识符）：请求的资源路径，通常包括主机名、端口号（如果非默认）、路径和查询字符串。
  * **HTTP 版本**：如 HTTP/1.1 或 HTTP/2。
  
  <mark>请求行的格式示例：`GET /index.html HTTP/1.1`</mark>
  <mark>Windows系统中，Enter相当于CRLF，回车符与换行符结合，保证在各种系统中能识别请求行</mark>

* **请求头**（Request Headers）：
  
  * 包含了客户端环境信息、请求体的大小（如果有）、客户端支持的压缩类型等。
  * 常见的请求头包括`Host`、`User-Agent`、`Accept`、`Accept-Encoding`、`Content-Length`等。

* **空行**：
  
  * 请求头和请求体之间的分隔符，表示请求头的结束。

* **请求体**（可选）：
  
  * 在某些类型的HTTP请求（如 POST 和 PUT）中，请求体包含要发送给服务器的数据。
    
    

##### 2.服务器响应信息

<mark>HTTP 响应也由四个部分组成，分别是：状态行、消息报头、空行和响应正文。</mark>

![httpmessage](https://www.runoob.com/wp-content/uploads/2013/11/httpmessage.jpg)



#### 2.HTTP请求方法：

| 序号  | 方法      | 描述                                                                             |
| --- | ------- | ------------------------------------------------------------------------------ |
| 1   | GET     | 从服务器获取资源。用于请求数据而不对数据进行更改。例如，从服务器获取网页、图片等。                                      |
| 2   | POST    | 向服务器发送数据以创建新资源。常用于提交表单数据或上传文件。发送的数据包含在请求体中。                                    |
| 3   | PUT     | 向服务器发送数据以更新现有资源。如果资源不存在，则创建新的资源。与 POST 不同，PUT 通常是幂等的，即多次执行相同的 PUT 请求不会产生不同的结果。 |
| 4   | DELETE  | 从服务器删除指定的资源。请求中包含要删除的资源标识符。                                                    |
| 5   | PATCH   | 对资源进行部分修改。与 PUT 类似，但 PATCH 只更改部分数据而不是替换整个资源。                                   |
| 6   | HEAD    | 类似于 GET，但服务器只返回响应的头部，不返回实际数据。用于检查资源的元数据（例如，检查资源是否存在，查看响应的头部信息）。                |
| 7   | OPTIONS | 返回服务器支持的 HTTP 方法。用于检查服务器支持哪些请求方法，通常用于跨域资源共享（CORS）的预检请求。                        |
| 8   | TRACE   | 回显服务器收到的请求，主要用于诊断。客户端可以查看请求在服务器中的处理路径。                                         |
| 9   | CONNECT | 建立一个到服务器的隧道，通常用于 HTTPS 连接。客户端可以通过该隧道发送加密的数据。                                   |



#### 3.HTTP响应头信息：

| 响应头信息（英文）                 | 响应头信息（中文） | 描述                                                                                                                                |
| ------------------------- | --------- | --------------------------------------------------------------------------------------------------------------------------------- |
| Date                      | 日期        | 响应生成的日期和时间。例如：Wed, 18 Apr 2024 12:00:00 GMT                                                                                       |
| Server                    | 服务器       | 服务器软件的名称和版本。例如：Apache/2.4.1 (Unix)                                                                                                |
| Content-Type              | 内容类型      | 响应体的媒体类型（MIME类型），如`text/html; charset=UTF-8`, `application/json`等。                                                                |
| Content-Length            | 内容长度      | 响应体的大小，单位是字节。例如：3145                                                                                                              |
| Content-Encoding          | 内容编码      | 响应体的压缩编码，如 `gzip`, `deflate`等。                                                                                                    |
| Content-Language          | 内容语言      | 响应体的语言。例如：zh-CN                                                                                                                   |
| Content-Location          | 内容位置      | 响应体的 URI。例如：/index.html                                                                                                           |
| Content-Range             | 内容范围      | 响应体的字节范围，用于分块传输。例如：bytes 0-999/8000                                                                                               |
| Cache-Control             | 缓存控制      | 控制响应的缓存行为, 如 no-cache 表示必须重新请求。                                                                                                   |
| Connection                | 连接        | 管理连接的选项，如`keep-alive`或`close`，keep-alive 表示连接不会在传输后关闭。。                                                                           |
| Set-Cookie                | 设置 Cookie | 设置客户端的 cookie。例如：sessionId=abc123; Path=/; Secure                                                                                 |
| Expires                   | 过期时间      | 响应体的过期日期和时间。例如：Thu, 18 Apr 2024 12:00:00 GMT                                                                                      |
| Last-Modified             | 最后修改时间    | 资源最后被修改的日期和时间。例如：Wed, 18 Apr 2024 11:00:00 GMT                                                                                    |
| ETag                      | 实体标签      | 资源的特定版本的标识符。例如："33a64df551425fcc55e6"                                                                                             |
| Location                  | 位置        | 用于重定向的 URI。例如：/newresource                                                                                                        |
| Pragma                    | 实现特定的指令   | 包含实现特定的指令，如 `no-cache`。                                                                                                           |
| WWW-Authenticate          | 认证信息      | 认证信息，通常用于HTTP认证。例如：Basic realm="Access to the site"                                                                               |
| Accept-Ranges             | 接受范围      | 指定可接受的请求范围类型。例如：bytes                                                                                                             |
| Age                       | 经过时间      | 响应生成后经过的秒数，从原始服务器生成到代理服务器。例如：24                                                                                                   |
| Allow                     | 允许方法      | 列出资源允许的 HTTP 方法 。例如：GET, POST，HEAD等                                                                                               |
| Vary                      | 变化        | 告诉下游代理如何使用响应头信息来确定响应是否可以从缓存中获取。例如：Accept                                                                                          |
| Strict-Transport-Security | 严格传输安全    | 指示浏览器仅通过 HTTPS 与服务器通信。例如：max-age=31536000; includeSubDomains                                                                      |
| X-Frame-Options           | 框架选项      | 控制页面是否允许在框架中显示，防止点击劫持攻击。例如：SAMEORIGIN                                                                                             |
| X-Content-Type-Options    | 内容类型选项    | 指示浏览器不要尝试猜测资源的 MIME 类型。例如：nosniff                                                                                                 |
| X-XSS-Protection          | XSS保护     | 控制浏览器的 XSS 过滤和阻断。例如：1; mode=block                                                                                                 |
| Public-Key-Pins           | 公钥固定      | HTTP 头信息，用于HTTP公共密钥固定（HPKP），一种安全机制，用于防止中间人攻击。例如：pin-sha256="base64+primarykey"; pin-sha256="base64+backupkey"; max-age=expireTime |



#### 4.HTTP状态码：

| 分类  | 分类描述                    |
| --- | ----------------------- |
| 1** | 信息，服务器收到请求，需要请求者继续执行操作  |
| 2** | 成功，操作被成功接收并处理           |
| 3** | 重定向，需要进一步的操作以完成请求       |
| 4** | 客户端错误，请求包含语法错误或无法完成请求   |
| 5** | 服务器错误，服务器在处理请求的过程中发生了错误 |

HTTP状态码列表:

| 状态码 | 状态码英文名称                         | 中文描述                                                                                                       |
| --- | ------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| 100 | Continue                        | 继续。客户端应继续其请求                                                                                               |
| 101 | Switching Protocols             | 切换协议。服务器根据客户端的请求切换协议。只能切换到更高级的协议，例如，切换到HTTP的新版本协议                                                          |
|     |                                 |                                                                                                            |
| 200 | OK                              | 请求成功。一般用于GET与POST请求                                                                                        |
| 201 | Created                         | 已创建。成功请求并创建了新的资源                                                                                           |
| 202 | Accepted                        | 已接受。已经接受请求，但未处理完成                                                                                          |
| 203 | Non-Authoritative Information   | 非授权信息。请求成功。但返回的meta信息不在原始的服务器，而是一个副本                                                                       |
| 204 | No Content                      | 无内容。服务器成功处理，但未返回内容。在未更新网页的情况下，可确保浏览器继续显示当前文档                                                               |
| 205 | Reset Content                   | 重置内容。服务器处理成功，用户终端（例如：浏览器）应重置文档视图。可通过此返回码清除浏览器的表单域                                                          |
| 206 | Partial Content                 | 部分内容。服务器成功处理了部分GET请求                                                                                       |
|     |                                 |                                                                                                            |
| 300 | Multiple Choices                | 多种选择。请求的资源可包括多个位置，相应可返回一个资源特征与地址的列表用于用户终端（例如：浏览器）选择                                                        |
| 301 | Moved Permanently               | 永久移动。请求的资源已被永久的移动到新URI，返回信息会包括新的URI，浏览器会自动定向到新URI。今后任何新的请求都应使用新的URI代替                                      |
| 302 | Found                           | 临时移动。与301类似。但资源只是临时被移动。客户端应继续使用原有URI                                                                       |
| 303 | See Other                       | 查看其它地址。与301类似。使用GET和POST请求查看                                                                               |
| 304 | Not Modified                    | 未修改。所请求的资源未修改，服务器返回此状态码时，不会返回任何资源。客户端通常会缓存访问过的资源，通过提供一个头信息指出客户端希望只返回在指定日期之后修改的资源                           |
| 305 | Use Proxy                       | 使用代理。所请求的资源必须通过代理访问                                                                                        |
| 306 | Unused                          | 已经被废弃的HTTP状态码                                                                                              |
| 307 | Temporary Redirect              | 临时重定向。与302类似。使用GET请求重定向                                                                                    |
|     |                                 |                                                                                                            |
| 400 | Bad Request                     | 客户端请求的语法错误，服务器无法理解                                                                                         |
| 401 | Unauthorized                    | 请求要求用户的身份认证                                                                                                |
| 402 | Payment Required                | 保留，将来使用                                                                                                    |
| 403 | Forbidden                       | 服务器理解请求客户端的请求，但是拒绝执行此请求                                                                                    |
| 404 | Not Found                       | 服务器无法根据客户端的请求找到资源（网页）。通过此代码，网站设计人员可设置"您所请求的资源无法找到"的个性页面                                                    |
| 405 | Method Not Allowed              | 客户端请求中的方法被禁止                                                                                               |
| 406 | Not Acceptable                  | 服务器无法根据客户端请求的内容特性完成请求                                                                                      |
| 407 | Proxy Authentication Required   | 请求要求代理的身份认证，与401类似，但请求者应当使用代理进行授权                                                                          |
| 408 | Request Time-out                | 服务器等待客户端发送的请求时间过长，超时                                                                                       |
| 409 | Conflict                        | 服务器完成客户端的 PUT 请求时可能返回此代码，服务器处理请求时发生了冲突                                                                     |
| 410 | Gone                            | 客户端请求的资源已经不存在。410不同于404，如果资源以前有现在被永久删除了可使用410代码，网站设计人员可通过301代码指定资源的新位置                                     |
| 411 | Length Required                 | 服务器无法处理客户端发送的不带Content-Length的请求信息                                                                         |
| 412 | Precondition Failed             | 客户端请求信息的先决条件错误                                                                                             |
| 413 | Request Entity Too Large        | 由于请求的实体过大，服务器无法处理，因此拒绝请求。为防止客户端的连续请求，服务器可能会关闭连接。如果只是服务器暂时无法处理，则会包含一个Retry-After的响应信息                       |
| 414 | Request-URI Too Large           | 请求的URI过长（URI通常为网址），服务器无法处理                                                                                 |
| 415 | Unsupported Media Type          | 服务器无法处理请求附带的媒体格式                                                                                           |
| 416 | Requested range not satisfiable | 客户端请求的范围无效                                                                                                 |
| 417 | Expectation Failed（预期失败）        | 服务器无法满足请求头中 Expect 字段指定的预期行为。                                                                              |
| 418 | I'm a teapot                    | 状态码 418 实际上是一个愚人节玩笑。它在 RFC 2324 中定义，该 RFC 是一个关于超文本咖啡壶控制协议（HTCPCP）的笑话文件。在这个笑话中，418 状态码是作为一个玩笑加入到 HTTP 协议中的。 |
|     |                                 |                                                                                                            |
| 500 | Internal Server Error           | 服务器内部错误，无法完成请求                                                                                             |
| 501 | Not Implemented                 | 服务器不支持请求的功能，无法完成请求                                                                                         |
| 502 | Bad Gateway                     | 作为网关或者代理工作的服务器尝试执行请求时，从远程服务器接收到了一个无效的响应                                                                    |
| 503 | Service Unavailable             | 由于超载或系统维护，服务器暂时的无法处理客户端的请求。延时的长度可包含在服务器的Retry-After头信息中                                                    |
| 504 | Gateway Time-out                | 充当网关或代理的服务器，未及时从远端服务器获取请求                                                                                  |
| 505 | HTTP Version not supported      | 服务器不支持请求的HTTP协议的版本，无法完成处理                                                                                  |



HTTP  content-type:[HTTP content-type | 菜鸟教程 (runoob.com)](https://www.runoob.com/http/http-content-type.html)

<mark>Content-Type（内容类型），一般是指网页中存在的 Content-Type，用于定义网络文件的类型和网页的编码，决定浏览器将以什么形式、什么编码读取这个文件，这就是经常看到一些 PHP 网页点击的结果却是下载一个文件或一张图片的原因。</mark>

<mark>Content-Type 标头告诉客户端实际返回的内容的内容类型。</mark>

<mark>语法格式：</mark>

```http
Content-Type: text/html; charset=utf-8
Content-Type: multipart/form-data; boundary=something
```

MIME类型：[MIME 类型 | 菜鸟教程 (runoob.com)](https://www.runoob.com/http/mime-types.html)

<mark>：MIME (Multipurpose Internet Mail Extensions) 是描述消息内容类型的标准，用来表示文档、文件或字节流的性质和格式。</mark>

<mark>MIME 消息能包含文本、图像、音频、视频以及其他应用程序专用的数据。</mark>

<mark>浏览器通常使用 MIME 类型（而不是文件扩展名）来确定如何处理URL，因此 We b服务器在响应头中添加正确的 MIME 类型非常重要。如果配置不正确，浏览器可能会无法解析文件内容，网站将无法正常工作，并且下载的文件也会被错误处理。</mark>





## HTTP Authorization（ [ˌɔθərəˈzeɪʃn]）：

### 一.基本了解：

#### 1.概念与功能：

HTTP Authorization 请求头是一种<mark>特殊的 HTTP 头部</mark>，允许客户端向服务器传达认证信息，格式如下：
    Authorization: <type> <credentials>

*它允许网络技术堆栈中的用户代理（例如，浏览器）向服务器展示认证信息（如令牌、用户名密码对），以完成身份验证过程。这一过程常在服务器需要确认发起请求的客户端是否具备资源访问权限时发生。*

#### 2.安全性：

**HTTP Authorization 头的安全使用至关重要，能够确保只有获得授权的用户才能够访问或操作敏感信息。它涵盖了从普通网页浏览到  API 调用等各种基于 HTTP 的活动。如果处理不当，如凭证信息被窃取，可能导致严重的安全后果，比如信息泄露或身份盗窃。**

**为了维护系统安全，重要的是要实施可靠的认证机制，确保认证信息在传输和处理过程中的安全性，以有效防止未经授权的访问和网络攻击（例如中间人攻击）。**

#### 3.五种主要认证机制：

* **<mark>基础认证</mark>**：尽管实现简单，但需要结合 HTTPS 使用以确保其安全性，因为其编码方式易于解码。
* **<mark>摘要认证</mark>**：通过使用比如 MD5 的散列函数来传输密码，其安全性高于基础认证，但在现代应用中相对较少使用。
* **<mark>令牌认证</mark>**（例如 Bearer 令牌）：作为一种更安全的认证方式，特别适用于 **[RESTful API](https://apifox.com/apiskills/rest-api/)**，并广泛用于现代网络应用中。
* **<mark>OAuth</mark>**：作为授权的开放标准，允许第三方应用安全地访问用户在另一服务提供商上的数据，无需用户直接向第三方提供凭证。
* **<mark>API 密钥</mark>**：虽易于使用，但因安全性相对较低，常结合其他安全措施一起使用，适用于安全要求不极高的场景。

### 二.认证机制：

#### 1.基础认证(Basic Authentication)：

BASIC 认证（基本认证）是从HTTP/1. 1 就定义的认证方式，是Web服务器与通信客户端之间进行的认证方式。

　　**BASIC 认证的步骤：**

![161544620200330172203423997372023](https://img2020.cnblogs.com/blog/1615446/202003/1615446-20200330172203423-997372023.png)

　步骤1：当请求的资源需要 BASIC 认证时，服务器会随状态码 **401 Authorization Required**，**返回 WWW-Authenticate 首部字段（响应头）的响应。该字段内包含认证的方式（BASIC）及Request-URI 安全域字符串（realm）**。

　　步骤2：接收到状态码 401 的客户端为了通过 BASIC 认证，需要将用户ID及密码发送给服务器。发送的字符串内容是由用户名 ID 和密码构成，两者中间以冒号（:）连接后，再经过 Base64 编码处理。

　　假设用户 ID 为 guest，密码是 guest，连接起来形成 "guest:guest" 这样的字符串。然后经过 Base64 编码成 Z3Vlc3Q6Z3Vlc3Q=。把这个字符串前面拼接 Baisc 形成 "Basic Z3Vlc3Q6Z3Vlc3Q="，然后把这个字符串写入首部字段 Authorization 后，发送请求。

　　当用户端为浏览器时，一个仅需要输入用户 ID 和密码即可，浏览器会自动完成 Base64 编码的工作，并再前面添加 "Basic" 前缀。

![](https://img2020.cnblogs.com/blog/1615446/202003/1615446-20200330173410894-884882819.png)

 　　步骤3：接收到包含首部字段 Authorization 请求的服务器，会对认证信息的正确性进行验证。如果验证通过，则返回一条包含 Request-URI 资源的响应。

<mark>**basic的缺点：**</mark>

**1）BASIC 认证虽然采用 Base64 密码方式，但这不是加密处理。不需要任何附加信息即可对其进行解码。换言之，由于明文解码后就是用户名 ID 和密码，再 HTTP 等非加密通信线路上进行 BASIC 认证的过程中，如果被人窃听，被盗的可能性极高。**

**所以，BASIC 认证需要配合HTTPS来保证信息传输的安全。**

**2）即使密码被强加密，第三方仍可通过加密后的用户名和密码进行重放攻击**

**3）如果想再进行一次 BASIC 认证时，一般的浏览器却无法实现认证注销操作。所以，BASIC 认证使用上不够灵活，且达不到多数 Web 网站期望的安全性等级**，因此并不常用。



#### 2.摘要认证（DIGEST Authentication）：

<mark> DIGEST 认证同样使用质询 / 响应的方式（challenge/response），但不会像 BASIC 认证那样直接发送明文密码。</mark>  
　　**所谓质询响应方式是指，一开始一方会先发送认证要求给另一方，接着使用从另一方那接收到的质询码计算生成响应码。最后将响应码返回给对方进行认证的方式。**

**DIGEST 认证的步骤：**

![](https://img2020.cnblogs.com/blog/1615446/202003/1615446-20200331162837843-77969056.png)

 　　**<mark>步骤 1： </mark>请求需认证的资源时，服务器会随着状态码 401Authorization Required，返回带WWW-Authenticate 首部字段的响应。该字段内包含质问响应方式认证所需的临时质询码（随机数，nonce）。首部字段 WWW-Authenticate 内必须包含realm 和nonce 这两个字段的信息。客户端就是依靠向服务器回送这两个值进行认证的。nonce 是一种每次随返回的 401 响应生成的任意随机字符串。该字符串通常推荐由Base64 编码的十六进制数的组成形式，但实际内容依赖服务器的具体实现。**



　**<mark>　步骤 2：</mark>接收到401状态码的客户端，返回的响应中包含 DIGEST 认证必须的首部字段 Authorization 信息。首部字段 Authorization 内必须包含 username、realm、nonce、uri 和response的字段信息。其中，realm 和 nonce 就是之前从服务器接收到的响应中的字段。  
　　username是realm 限定范围内可进行认证的用户名。uri（digest-uri）即Request-URI的值，但考虑到经代理转发后Request-URI的值可能被修改因此事先会复制一份副本保存在 uri内。response 也可叫做 Request-Digest，存放经过 MD5 运算后的密码字符串，形成响应码。**



　**<mark>　步骤 3：</mark>接收到包含首部字段 Authorization 请求的服务器，会确认认证信息的正确性。认证通过后则返回包含 Request-URI 资源的响应。并且这时会在首部字段 Authentication-Info 写入一些认证成功的相关信息。（不过我下面的例子没有去写这个Authentication-Info，而是直接返回的数据。因为我实在session里缓存的认证结果）。**



补充：

![](file:///C:/Users/%E6%99%93/Pictures/Saved%20Pictures/QQ20240805-105451.png)



#### 3.OAuth授权认证(/'əu 'ɔːθ/):

##### 1.通俗理解：

[OAuth 2.0 的一个简单解释 - 阮一峰的网络日志 (ruanyifeng.com)](https://www.ruanyifeng.com/blog/2019/04/oauth_design.html<mark>)

<mark>：这里用了一个小区-外卖员的例子</mark>

<mark>**简单说，OAuth 就是一种授权机制。数据的所有者告诉系统，同意授权第三方应用进入系统，获取这些数据。系统从而产生一个短期的进入令牌（token），用来代替密码，供第三方应用使用。**</mark>



##### 2.令牌与密码：

<mark>令牌（token）与密码（password）的作用是一样的，都可以进入系统，但是有三点差异。</mark>

**（1）令牌是短期的，到期会自动失效，用户自己无法修改。密码一般长期有效，用户不修改，就不会发生变化。**

**（2）令牌可以被数据所有者撤销，会立即失效。以上例而言，屋主可以随时取消快递员的令牌。密码一般不允许被他人撤销。**

**（3）令牌有权限范围（scope），比如只能进小区的二号门。对于网络服务来说，只读令牌就比读写令牌更安全。密码一般是完整权限。**



<mark>ps：OAuth是什么，可以去看RFC 6749文件，这里有几句话...........</mark>

> <mark>OAuth 引入了一个授权层，用来分离两种不同的角色：客户端和资源所有者。......资源所有者同意以后，资源服务器可以向客户端颁发令牌。客户端通过令牌，去请求数据。</mark>

这段话的意思就是，**OAuth 的核心就是向第三方应用颁发令牌**。然后，RFC 6749 接着写道：

> <mark>（由于互联网有多种场景，）本标准定义了获得令牌的四种授权方式（authorization grant ）。</mark>

也就是说，**OAuth 2.0 规定了四种获得令牌的流程。你可以选择最适合自己的那一种，向第三方应用颁发令牌**。下面就是这四种授权方式。

> * <mark>授权码（authorization-code）</mark>
> * <mark>隐藏式（implicit）</mark>
> * <mark>密码式（password）：</mark>
> * <mark>客户端凭证（client credentials）</mark>

注意，不管哪一种授权方式，第三方应用申请令牌之前，都必须先到系统备案，说明自己的身份，然后会拿到两个身份识别码：客户端 ID（client ID）和客户端密钥（client secret）。这是为了防止令牌被滥用，没有备案过的第三方应用，是不会拿到令牌的。

<mark>：四种授权模式，可以查看[OAuth 2.0 授权认证详解-CSDN博客](https://blog.csdn.net/fuhanghang/article/details/131394196)和[OAuth 2.0 的四种方式 - 阮一峰的网络日志 (ruanyifeng.com)](https://www.ruanyifeng.com/blog/2019/04/oauth-grant-types.html)；</mark>

<mark>ps：我就学个通用的授权码模式^__^</mark>



##### 3.授权码模式：

**授权码模式（authorization code）是功能最完整、流程最严密安全的授权模式。它的特点就是<u>通过客户端的<strong>后台服务器</strong>，与"服务提供商"的认证服务器进行互动。</u>**

**（这种方式适用于那些有后端的 Web 应用。<u>授权码通过前端传送，令牌则是储存在后端</u>，而且所有与资源服务器的通信都在后端完成。这样的前后端分离，可以避免令牌泄漏。）**

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/13cfde787ac315805c9c65ec0b920a06.png)

![](file:///C:/Users/%E6%99%93/Pictures/Saved%20Pictures/QQ%E6%88%AA%E5%9B%BE20240805140432.png)

**<mark>授权码模式流程如下：</mark>**

1）用户访问客户端，客户端将用户导向认证服务器。

2）用户选择是否给予客户端授权。

3）假设用户给予授权，认证服务器将用户导向客户端事先指定的"重定向URI"（redirection URI），同时附上一个授权码（每个用户的授权码不同）。

4）客户端收到授权码，附上早先的"重定向URI"，向认证服务器申请令牌。这一步是在客户端的 **后台服务器** 上完成的，对用户不可见。

5）认证服务器核对了授权码和重定向URI，确认无误后，向客户端发送访问令牌（access token）和更新令牌（refresh token）。

<mark>*从上述的流程描述可知，只有第 2 步需要用户进行授权操作，之后的流程都是在客户端的后台和认证服务器后台之前进行"静默"操作，对于用户来说是无感知的。*</mark>



##### 4.授权码模式流程详解：

第 1 步骤中，客户端申请认证的URI，包含以下参数：

* `response_type`：表示授权类型，**必选项**，此处的值固定为"code"
* `client_id`：表示客户端的ID，**必选项**
* `redirect_uri`：表示重定向URI，可选项
* `scope`：表示申请的权限范围，可选项
* `state`：表示客户端的当前状态，可以指定任意值，认证服务器会原封不动地返回这个值。

**示例**

A 网站提供一个链接，用户点击后就会跳转到 B 网站，授权用户数据给 A 网站使用。下面就是 A 网站跳转 B 网站的一个示意链接：

```http
https://b.com/oauth/authorize?response_type=code&client_id=CLIENT_ID&redirect_uri=CALLBACK_URL&scope=read
```

上面 URL 中：

<mark>`response_type`</mark>参数表示要求返回授权码（`code`）；

<mark><code>client_id</code></mark>参数让 B 网站知道是谁在请求；

<mark>redirect_uri</mark>参数是 B 网站接受或拒绝请求后的跳转网址；

<mark>scope</mark>参数表示要求的授权范围（这里是只读）。



**第 2 步骤**

第 2 步骤中，<u>用户跳转后，</u><u>B 网站会要求用户登录，然后询问是否同意给予 A 网站授权</u>。



**第 3 步骤**

**参数说明**

第 3 步骤中，服务器回应客户端的URI，包含以下参数：

* `code`：表示授权码，**必选项**。该码的有效期应该很短，<u>通常设为10分钟，客户端只能使用该码一次，否则会被授权服务器拒绝。</u>该码与客户端ID和重定向URI，是一一对应关系。
* `state`：如果客户端的请求中包含这个参数，认证服务器的回应也必须一模一样包含这个参数。

**示例**

在第 2 步骤用户表示同意之后，这时 B 网站就会跳回`redirect_uri`参数指定的网址。跳转时，会传回一个授权码，就像下面这样。

```http
https://a.com/callback?code=AUTHORIZATION_CODE
```

上面 URL 中，`code`参数就是授权码。



**第 4 步骤**

**参数说明**

第 4 步骤中，客户端向认证服务器申请令牌的HTTP请求，包含以下参数：

* `grant_type`：表示使用的授权模式，**必选项**，此处的值固定为"authorization_code"。
* `code`：表示上一步获得的授权码，**必选项**。
* `redirect_uri`：表示重定向URI，**必选项**，且必须与A步骤中的该参数值保持一致。
* `client_id`：表示客户端ID，**必选项**。

**示例**

在第 3 步骤中，A 网站拿到授权码以后，就可以在后端，向 B 网站请求令牌。

```http
https://b.com/oauth/token? client_id=CLIENT_ID& client_secret=CLIENT_SECRET& grant_type=authorization_code& code=AUTHORIZATION_CODE& redirect_uri=CALLBACK_URL
```

上面 URL 中：

<mark>`client_id`</mark>参数和<mark>`client_secret`</mark>参数用来让 B 确认 A 的身份（<mark>`client_secret`</mark>参数是保密的，因此只能在后端发请求）；

<mark>`grant_type`</mark>参数的值是<mark>`AUTHORIZATION_CODE`</mark>，表示采用的授权方式是授权码；

<mark>`code`</mark>参数是上一步拿到的授权码；

<mark>`redirect_uri`</mark>参数是令牌颁发后的回调网址。



**第 5 步骤**

**参数说明**

第 5 步骤中，认证服务器发送的HTTP回复，包含以下参数：

* `access_token`：表示访问令牌，必选项。
* `token_type`：表示令牌类型，该值大小写不敏感，必选项，可以是bearer类型或mac类型。
* `expires_in`：表示过期时间，单位为秒。如果省略该参数，必须其他方式设置过期时间。
* `refresh_token`：表示更新令牌，用来获取下一次的访问令牌，可选项。
* `scope`：表示权限范围，如果与客户端申请的范围一致，此项可省略。

**示例**

第 4 步骤中，B 网站收到请求以后，就会颁发令牌。具体做法是向`redirect_uri`指定的网址，发送一段 JSON 数据：

```http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache

{"access_token":"ACCESS_TOKEN","token_type":"bearer","expires_in":2592000,"refresh_token":"REFRESH_TOKEN","scope":"read","uid":100101,"info":{...}
} 
```

上面 JSON 数据中，`access_token`字段就是令牌，A 网站在后端拿到了。注意：HTTP头信息中明确指定不得缓存。****

**：其他模式-哔哩哔哩IT小齐-理解**





### 三.JWT跨域认证：



##### 1.jwt原理：

     JWT 的原理是，服务器认证以后，生成一个 JSON 对象，发回给用户，就像下面这样。

> ```json
> {
>   "姓名": "张三",
>   "角色": "管理员",
>   "到期时间": "2018年7月1日0点0分"
> }
> ```

以后，用户与服务端通信的时候，都要发回这个 JSON 对象。服务器完全只靠这个对象认定用户身份。为了防止用户篡改数据，服务器在生成这个对象的时候，会加上签名

服务器就不保存任何 session 数据了，也就是说，服务器变成无状态了，从而比较容易实现扩展。



##### 2.jwt数据结构：

**实际的 JWT 大概就像下面这样。**

![](https://cdn.beekka.com/blogimg/asset/201807/bg2018072304.jpg)

<mark>它是一个很长的字符串，中间用点（`.`）分隔成三个部分。注意，JWT 内部是没有换行的，这里只是为了便于展示，将它写成了几行。</mark>

JWT 的三个部分依次如下。

> * <mark>Header（头部）</mark>
> * <mark>Payload（负载）</mark>
> * <mark>Signature（签名）</mark>

写成一行，就是下面的样子。

> ```http
> Header.Payload.Signature
> ```

![](https://cdn.beekka.com/blogimg/asset/201807/bg2018072303.jpg)



###### 2.1 Header

Header 部分是一个 JSON 对象，描述 JWT 的元数据，通常是下面的样子。

> ```json
> {
>   "alg": "HS256",
>   "typ": "JWT"
> }
> ```

上面代码中，`alg`属性表示签名的算法（algorithm），默认是 HMAC SHA256（写成 HS256）；`typ`属性表示这个令牌（token）的类型（type），JWT 令牌统一写为`JWT`。

最后，将上面的 JSON 对象使用 Base64URL 算法（详见后文）转成字符串。



###### 2.2 Payload

Payload 部分也是一个 JSON 对象，用来存放实际需要传递的数据。JWT 规定了7个官方字段，供选用。

> * <mark>iss (issuer)：签发人</mark>
> * <mark>exp (expiration time)：过期时间</mark>
> * <mark>sub (subject)：主题</mark>
> * <mark>aud (audience)：受众</mark>
> * <mark>nbf (Not Before)：生效时间</mark>
> * <mark>iat (Issued At)：签发时间</mark>
> * <mark>jti (JWT ID)：编号</mark>

除了官方字段，你还可以在这个部分定义私有字段，下面就是一个例子。

> ```json
> {
>   "sub": "1234567890",
>   "name": "John Doe",
>   "admin": true
> }
> ```

<mark>注意，JWT 默认是不加密的，任何人都可以读到，所以不要把秘密信息放在这个部分。</mark>

这个 JSON 对象也要使用 Base64URL 算法转成字符串。



###### 2.3 Signature

Signature 部分是对前两部分的签名，防止数据篡改。

首先，需要指定一个密钥（secret）。这个密钥只有服务器才知道，不能泄露给用户。然后，使用 Header 里面指定的签名算法（默认是 HMAC SHA256），按照下面的公式产生签名。

> ```http
> HMACSHA256(
>   base64UrlEncode(header) + "." +
>   base64UrlEncode(payload),
>   secret)
> ```

算出签名以后，把 Header、Payload、Signature 三个部分拼成一个字符串，每个部分之间用"点"（`.`）分隔，就可以返回给用户。



###### 2.4 Base64URL

前面提到，Header 和 Payload 串型化的算法是 Base64URL。这个算法跟 Base64 算法基本类似，但有一些小的不同。

JWT 作为一个令牌（token），有些场合可能会放到 URL（比如 api.example.com/?token=xxx）。Base64 有三个字符`+`、`/`和`=`，在 URL 里面有特殊含义，所以要被替换掉：`=`被省略、`+`替换成`-`，`/`替换成`_` 。这就是 Base64URL 算法。



#### 3、JWT 的使用方式

客户端收到服务器返回的 JWT，可以储存在 Cookie 里面，也可以储存在 localStorage。

此后，客户端每次与服务器通信，都要带上这个 JWT。你可以把它放在 Cookie 里面自动发送，但是这样不能跨域，所以更好的做法是放在 HTTP 请求的头信息`Authorization`字段里面。

> ```http
> Authorization: Bearer <token>
> ```

另一种做法是，跨域的时候，JWT 就放在 POST 请求的数据体里面。


