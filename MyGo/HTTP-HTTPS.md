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


