---
Web 优化 “黑魔法” 的动机与方式
---

### 目录

1. 关键性能指标
2. HTTP/1 的问题
3. Web 性能优化最佳实践

### 关键性能指标

指标：延迟、带宽。

#### 延迟

延迟是指 IP 数据包从一个网络端点到另一个网络端点所花费的时间。与之相关的是往返时延（RTT），它是延迟的时间的两倍。延迟是制约 Web 性能的主要瓶颈，尤其对于 HTTP 这样的协议，因为其中包含大量往返于服务器的请求。

#### 带宽

只要带宽没有饱和，两个网络端点之间的连接会一次处理尽可能多的数据量。依据 Web 页面引用资源的大小和网络连接的传输能力，带宽可能会成为性能的瓶颈。

### HTTP/1 的问题

问题：队头阻塞、低效的 TCP 利用、臃肿的首部和受限的优先级设置。

#### 队头阻塞

浏览器很少只从一个域名获取一份资源。大多数时候，它希望能同时获取许多资源。设想这样一个网站，它把所有图片放在单个特定的域名下。HTTP/1 并未提供机制来同时请求这些资源。如果仅仅使用一个连接，它需要发起请求，等待响应，之后才能发起下一个请求。h1 有个特性叫管道化，允许一次发送一组请求，但是只能按照发送顺序依次接收响应。而且，管道化备受互操作性和部署的各种问题的困扰，基本没有实用价值。

在请求应答过程中，如果出现任何状况，剩下所有的工作都会被阻塞在那次请求应答之后。这就是 ”队头阻塞“，它会阻塞网络传输和 Web 页面渲染，直至失去响应。为了防止这种问题，现代浏览器会针对单个域名开启 6 个连接，通过各个连接分别发送请求。它实现了某种程度上的并行，但是每个连接仍会受到 ”队头阻塞“ 的影响。

#### 低效的 TCP 利用

传输控制协议（TCP）的设计思路是：对假设情况很保守，并能够公平对待同一网络的不同流量的应用。它的避免拥塞机制被设计成即使在最差的网络状况下仍能起作用，并且如果有需求冲突也保证相对公平。这是它取得成功的原因之一，它的成功并不是因为传输数据最快，而是因为它是最可靠的协议之一，涉及的核心概念就是拥塞窗口。拥塞窗口是指，在接收方确认数据包之前，发送方可以发出的 TCP 包的数量。例如，如果拥塞窗口指定为 1，那么发送方发出 1 个数据包之后，只有接收方确认了那么包，才能发送下一个。

一般来讲，每次发送一个数据包并不是非常低效。TCP 有个概念就慢启动，它用来探索当前连接对应拥塞窗口的合适大小。慢启动的设计目标是为了让新连接搞清楚当前网络状况，避免给已经拥堵的网络继续添乱。它允许发送者在收到每个确认回复后额外发送 1 个未确认包。这意味着新连接在收到 1 个确认回复之后，可以发送 2 个数据包；在收到 2 个确认回复后，可以发 4 个；以此类推，这种几何级数增长很快就会达到协议规定的发包数上限，这时候连接将进入拥塞避免阶段。

![TCP 拥塞控制.png](https://i.loli.net/2019/11/11/vWPindHfIFsyXB8.png)

这种机制需要几次往返数据请求才能得知最佳拥塞窗口大小。但在解决性能问题时，就这区区几次数据往返也是非常宝贵的时间成本。现代操作系统一般会取 4~10 个数据包作为初始拥塞窗口大小。如果你把一个数据包设置为最大值下限 1460 字节（也就是最大有效负载），那么只能先发送 5840 字节（假定拥塞窗口为 4），然后就需要等待接受确认回复。如今的 Web 页面平均大小约 2MB，包括 HTML 和所有依赖的资源。在理想情况下，这需要大约九次往返请求来传输完整个页面。除此之外，浏览器一般会针对用一个域名开启 6 个并发连接，每个连接都免不了拥塞窗口调节。

前面提到过，因为 h1 并不支持多路复用，所以浏览器一般会针对指定域名开启 6 个并发连接。这意味着拥塞窗口波动也会并行发生 6 次。TCP 协议保证那些连接都能正常工作，但是不能保证它们的性能是最优的。

#### 臃肿的消息首部

虽然 h1 提供了压缩被请求内容的机制，但是消息首部却无法压缩。消息首部可不能忽略，尽管它比响应资源小很多，但它可能占据请求的绝大部分，如果算上 Cookie，有几千字节就很正常了。

#### 受限的优先级设置

如果浏览器针对指定域名开启了多个 Socket，开始请求资源，这时候浏览器能指定优先级的方式是有限的：要么发起请求，要么不发起。然后 Web 页面上某些资源会比另一些更重要，这必然会加重资源的排队效应。这是因为浏览器为了先请求优先级高的资源，会推迟请求其他资源。但是优先级高的资源获取之后，在处理的过程中，浏览器并不会发起新的资源请求，所以浏览器无法利用这段时间发送优先级低的资源，总的页面下载时间因此延长了。

### Web 性能优化最佳实践

参见《Web 性能优化最佳实践》。