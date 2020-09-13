---
layout: post_layout
title:  "WebSocket"
date:   2019-04-10 20:29PM
categories: jekyll update
type: 2
summary: "长期以来，在web领域中，若想让客户端与服务端交互，我们首（或者说唯一的）选的肯定是Http。而随着web应用的快速发展，数据的消费量和功能需求的强度也逐渐增加，很显然，传统的Http模式早已不能满足我们..."
icon: "js-icon.jpg"
---

### 1.开篇

> 以下为我之前給组内分享的有关WebSocket的话题，略有删减，未完待续。。。

![001](/../img/websocket/websocket.001.jpeg)

长期以来，在web领域中，若想让客户端与服务端交互，我们首（或者说唯一的）选的肯定是Http。而随着web应用的快速发展，数据的消费量和功能需求的强度也逐渐增加，很显然，传统的Http模式早已不能满足我们，更多的需求则希望服务端可以“主动”跟客户端通信来增加交互。不得已，long-polling逐渐的被实现在服务端，而这仍然是一种十分被动的策略。

![002](/../img/websocket/websocket.002.jpeg)

传统的Http很简单：客户端主动发起请求，服务端接收到请求后立刻返回结果，一次“单向且即时的交流”就此完成。

![003](/../img/websocket/websocket.003.jpeg)

那么问题来了：如果客户端不请求，又想从服务端获取信息，那该怎么办呢？

试想下以下场景：

1.你的好友发了条Facebook，你想回复些什么，正当你typing的时候，发现突然多个一个“赞”，如果你侥幸利用Network记录了此过程，你会诧异的发现，并没有什么请求发送，那么这个“赞”又是如何更新到你的页面的呢？

2.你修复了一个bug，git操作行云流水，提交代码后触发了流水线，你倒杯茶静静的等待部署的结果，lint-->build-->test-->deploy，这一连串的状态更新与你无关，它就像一个信号接收器一样一直接收着来后端的信息，这又是如何做到的呢？

其实web交互还有一些其他方式，下面的一些介绍，也许会让你对上面的疑问有所解答。

![004](/../img/websocket/websocket.004.jpeg)

Polling Request其实也是Http请求，它不停的循环，使得在一些场景下，让用户误以为它是“一直”在那里工作：譬如如果对准确度要求不高，一些进度条或者loading状态的更新就可以采用这种办法，因为用户在意的不是准确度，而是“它是否在工作”，以此来增强体验。但大部分情况下，它都是傻瓜式的，轮训的时间段和处理逻辑的时间一旦冲突，结果往往很难预料。

![005](/../img/websocket/websocket.005.jpeg)

Long-Polling Request最大的差别是在于当客户端将请求发給服务端后，服务端并没有立刻返回，而是一直持有着这次交互，一直等到服务端真正的获取到信息之后，才会将结果返回給客户端。因此从表象看来，似乎客户端已经发送了请求，而页面没有立刻更新，那就代表结果为空，以为这次请求交互已经结束，而过一会，突然又更新了，“感觉”像是服务端主动推送了数据，而实际上只是一次“延迟”的交互。

![006](/../img/websocket/websocket.006.jpeg)

EventSource顾名思义，在客户端与服务端交互的过程中，是以事件进行传递的，更确切的说，它是单向的，但它和传统Http恰好相反。客户端和服务端一旦建立起链接，事件只能由服务端向客户端推送。不难看出，它非常适用于客户端需要被动实时频繁更新信息的交互场景。如果你利用Network来调试，也是能够清晰的看到event stream的。

![007](/../img/websocket/websocket.007.jpeg)

可有没有什么办法可以既可以让客户端从服务端获取信息，且反之亦然呢？

![008](/../img/websocket/websocket.008.jpeg)

不卖关子了，主角登场：WebSocket

与Http不同，WebSocket可以在没有客户端请求的情况下让服务端主动給客户端发送数据，客户端和服务端可以实时地进行双向通信，因此它非常适合“即时在线”的沟通。

而在通信安全方面，WebSocket有着自己独立的加密协议`wss://`，与Http类似，`ws://`并不被推荐，原因不言自明。

WebSocket的使用也十分简单，客户端建立链接如下：

{% highlight javascript %}

const url = 'wss://myserver.com/something'

const connection = new WebSocket(url)

{% endhighlight %}

此时便可以订阅相关事件：

{% highlight javascript %}

connection.onopen = () => {}

connection.onerror = error => {}

{% endhighlight %}



![009](/../img/websocket/websocket.009.jpeg)

用之前呢，我们还是最好看一下[can I use](http://www.caniuse.com/)


![010](/../img/websocket/websocket.010.jpeg)

WebSocket的服务端实现的版本非常多，以Node.js为例，代码片段如下：

{% highlight sh %}

npm i ws

{% endhighlight %}

{% highlight javascript %}

const WebSocket = require('ws')

const wss = new WebSocket.Server({ port: 8080 })

wss.on('connection', ws => {
  ws.on('message', message => {
    console.log(`Received message => ${message}`)
  })
  ws.send('hello world!')
})

{% endhighlight %}


![012](/../img/websocket/websocket.012.jpeg)

如果你有尝试以上介绍做点小demo的话，你会发现，利用Network是可以看到所有明文的信息的，这种“暴露性”可能让你感到不安。而如果你又很好奇的去看看某弹幕平台的WebSocket信息，你会发现完全是加密，这一点和传统的Socket通信也是类似的，这些信息其实都是经过了某些算法进行二进制化，但其实都是有固定的格式和校验码的。

![013](/../img/websocket/websocket.013.jpeg)

带着这点好奇心，留下最后一个问题：如何获取到某直播平台的弹幕（使用WebSocket）数据包呢？且如果WebSocket的信息又被二进制化（或者加密）了呢？

![014](/../img/websocket/websocket.014.jpeg)

To be continue...
