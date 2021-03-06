## 跨域
### 什么是同源策略
源(orgin)就是协议、域名和端口号。同源指的就是协议相同、域名相同、端口相同。

### JSONP
**原理：**  
利用 script 标签实现，因为不管哪个网站上的 js 文件，只要引入了就可以去运行，所以不受同源策略的影响。

**实现：**  
当需要通讯的时候，本地创建一个 script 元素，地址指向 api 网址，并提供一个回调函数来接受数据（与后端约定函数名）。

### CORS 跨域资源共享
只需要需要在服务器端设置响应头

**原理：** 
CORS原理只需要向响应头header中注入Access-Control-Allow-Origin，这样浏览器检测到header中的Access-Control-Allow-Origin，则就可以跨域操作了。

### PostMessage
### Nginx 反向代理

## cookie 是什么？session 是什么？
Session 是在服务端保存的一个数据结构，用来跟踪用户的状态。

Cookie 是客户端保存用户信息的一种机制，用来记录用户的一些信息。

## GET 和 POST 的区别
- GET 会限制发送的长度，POST 不会
- GET 通过 URL 传递，而 POST 则是放在 RequestBody 中
- GET 在游览器退回时是无害的，而 POST 则会再次请求

## 前端缓存如何实现
通过在服务器端设置请求头来实现游览器缓存。

- 设置 Expires（当 Cache-control 会忽略 Expires）
- 设置 Cache-control: max-age
- 设置 Etag

## 浏览器缓存有哪些

浏览器缓存分为强缓存和协商缓存。  

强缓存：浏览器在加载资源时，首先根据资源的HTTP Header判断是否命中强缓存，如果命中，则不会发送请求到服务器。  

协商缓存：如果强缓存没有命中，服务器一定会发送一个请求到服务器。通过服务端一些其他的一些HTTP Header去判断是否命中协商缓存，如果命中，服务器会将这个请求返回（304）。但不会返回数据，告诉客户端可以从缓存中加载这个资源，若未命中，则返回数据，并更新本地缓存。  

设置缓存通常通过meta标签和HTTP头信息，meta标签主要通过pragma no-cache控制，HTTP Header主要通过Expires（强缓存）、Cache-control（强缓存）、Last-Modified/If-Modified-Since（协商缓存）、Etag/If-None-Match（协商缓存）实现。

## HTTP 协议状态码
- 200 请求成功
- 206 客户端请求范围数据，服务器返回
- 301 请求的页面转移到了新的 URL
- 302 请求的页面临时转移到了新的 URL
- 304 客户端有缓存文件，并发出了一个请求，服务器告诉客户端，缓存可以继续使用
- 400 客户端请求语法错误
- 401 请求未经授权
- 403 访问被禁止
- 404 所请求页面不存在
- 500 服务器发生错误
- 503 服务器宕机或者过载

## HTTP 和 HTTPS 的区别
- HTTPS 需要到 CA 申请证书
- HTTP 是明文传输，HTTPS 是有安全性的 SSL 加密传输协议
- HTTP 使用 80 端口，HTTPS 使用 443 端口
- HTTP 的连接是无状态的，https是ssl加密的传输，身份认证的网络协议，相对http传输比较安全

HTTP + 加密 + 认证 + 保护 = HTTPS

### SSL 加密
TODO

## HTTP 1.1 和 HTTP 2.0
- HTTP/2采用二进制格式而非文本格式
- HTTP/2是完全多路复用的，而非有序并阻塞的——只需一个连接即可实现并行
  - 多路复用允许同时通过单一的 HTTP/2 连接发起多重的请求-响应消息
- 使用报头压缩，HTTP/2降低了开销
- HTTP/2让服务器可以将响应主动“推送”到客户端缓存中（比如推送 js文件、css样式、字体等）

## http 响应中 Content-type 包含哪些内容
包含请求中的消息主体是用何种方式编码（简单的说就是传输信息的格式），例如：
- text/html
- imgage/..
- application/...

application/x-www-form-urlencoded  
这是最常见的 POST 提交数据的方式 按照 key1=val1&key2=val2 的方式进行编码

application/json  
告诉服务端消息主体是序列化后的 JSON 字符串

## https 过程
1. 客户端发送请求到服务器端
2. 服务器端返回证书和公开密钥，公开密钥作为证书的一部分而存在
3. 客户端验证证书和公开密钥的有效性，如果有效，则生成共享密钥并使用公开密钥加密发送到服务器端
4. 服务器端使用私有密钥解密数据，并使用收到的共享密钥加密数据，发送到客户端
5. 客户端使用共享密钥解密数据
6. SSL加密建立………

## JWT-token
TODO

## TCP/IP 分层模型
参考：https://blog.csdn.net/be_happy_mr_li/article/details/52243006

TCP/IP 模型分为 5层：应用层、传输层、网络层、数据链路层以及 物理层

### 应用层
应用层是我们经常接触使用的部分，比如常用的http协议、ftp协议（文件传输协议）、snmp（网络管理协议）、telnet （远程登录协议 ）、smtp（简单邮件传输协议）、dns（域名解析），这主要是面向用户的交互的。这里的应用层集成了osi分层模型中 的应用、会话、表示层三层的功能。

### 传输层
传输层的作用就是将应用层的数据进行传输转运。比如我们常说的tcp（可靠的传输控制协议）、udp（用户数据报协议）。传输单位为报文段。

*tcp（Transmission Control Protocol）*  
面向连接（先要和对方确定连接、传输结束需要断开连接，类似打电话）、复杂可靠的、有很好的重传和查错机制。一般用与高速、可靠的通信服务。

*udp（user datagram protocol）*  
面向无连接（无需确认对方是否存在，类似寄包裹）、简单高效、没有重传机制。一般用于即时通讯、广播通信等。