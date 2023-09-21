## HTTP/1.0

One of the main disadvantages of  HTTP/1.0 is that each TCP connection can only contain just one request. 

The 3-Way handshake of TCP is costly, so someone uses `Connection: keep-alive` configuration to maintain TCP connections. However, it's a tricky way, and can not be standardizable.

The first response that a client receives on an HTTP `GET` request is often not the fully rendered page. Instead, it contains links to additional resources needed by the requested page. Because of this, the client will have to make additional requests to retrieve these resources. 

## HTTP/1.1

### persistent connection and pipelining

HTTP/1.1 introduces persistent connections and pipelining, which take the place of `Connectin: keep-alive` configuration. With persistent connections, HTTP/1.1 assumes that a TCP connection should be kept open unless directly told to close(by using `Connectin: close`). This allows the client to send multiple requests along the same connection without waiting for a response to each, greatly improving the performance of HTTP/1.1 over HTTP/1.0.

###  Content-Length

With the help of pipelining, now a TCP connection can transmit multi requests. It's necessary to figure out which request a data package belongs to. The `Content-Length` parameter declares the length of data. 

In some situations using data streams, it's hard to calculate data length. For this reason, `Content-Length` is not necessary for version 1.1. `Transfer-Encoding` is used to declare requests which contain data with uncertain length.

### more functions

- PUT
- PATCH
- HEAD
- OPTIONS
- DELETE

### defect

Though it's possible to reuse TCP connections, all requests in one connection have a sequence. If one request is pretty slow, requests behind will be blocked.

### HTTP/2

In HTTP/1.1, the header type must be ASCII, body type can be ASCII or binary. HTTP/2 is a total binary protocol, both header and body must be encoded to binary, in order to prepare for further functions.

---

HTTP/2 

HTTP/2 复用TCP连接，在一个连接里，客户端和浏览器都可以同时发送多个请求或回应，而且不用按照顺序一一对应，这样就避免了"队头堵塞"。

举例来说，在一个TCP连接里面，服务器同时收到了A请求和B请求，于是先回应A请求，结果发现处理过程非常耗时，于是就发送A请求已经处理好的部分， 接着回应B请求，完成后，再发送A请求剩下的部分。

这样双向的、实时的通信，就叫做多工（Multiplexing）。

---

因为 HTTP/2 的数据包是不按顺序发送的，同一个连接里面连续的数据包，可能属于不同的回应。因此，必须要对数据包做标记，指出它属于哪个回应。

HTTP/2 将每个请求或回应的所有数据包，称为一个数据流（stream）。每个数据流都有一个独一无二的编号。数据包发送的时候，都必须标记数据流ID，用来区分它属于哪个数据流。另外还规定，客户端发出的数据流，ID一律为奇数，服务器发出的，ID为偶数。

数据流发送到一半的时候，客户端和服务器都可以发送信号（`RST_STREAM`帧），取消这个数据流。1.1版取消数据流的唯一方法，就是关闭TCP连接。这就是说，HTTP/2 可以取消某一次请求，同时保证TCP连接还打开着，可以被其他请求使用。

客户端还可以指定数据流的优先级。优先级越高，服务器就会越早回应。

---

HTTP 协议不带有状态，每次请求都必须附上所有信息。所以，请求的很多字段都是重复的，比如`Cookie`和`User Agent`，一模一样的内容，每次请求都必须附带，这会浪费很多带宽，也影响速度。

HTTP/2 对这一点做了优化，引入了头信息压缩机制（header compression）。一方面，头信息使用`gzip`或`compress`压缩后再发送；另一方面，客户端和服务器同时维护一张头信息表，所有字段都会存入这个表，生成一个索引号，以后就不发送同样字段了，只发送索引号，这样就提高速度了。

