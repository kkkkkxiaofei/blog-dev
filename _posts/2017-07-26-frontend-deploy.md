---
layout: post_layout
title:  "前端项目应该如何部署"
date:   2017-07-26 18:03PM
categories: jekyll update
type: 2
summary: "一个标准的前端项目，必定始于yarn start，这一流程必定经历babel编译，webpack构建，server启动，然后由浏览器加载页面。这是很Dev的开发方式，可生产环境我们却往往不这么做。"
icon: "js-icon.jpg"
---

>一个标准的前端项目，必定始于yarn start，这一流程必定经历babel编译，webpack构建，server启动，然后由浏览器加载页面。这是很Dev的开发方式，可生产环境我们却往往不这么做。

### 1.何为前端？

如果按照以前的看法，前后端最本质的区别当然是运行环境了，一个是浏览器中`所写即所见`的UI界面，另一个则是`藏在背后`的服务。

在这种简单的区分下，前端往往会被定义为`HTML/CSS/Javascript`。没错，前端就是这些东西，可也不能只有这些东西：前端有时也需要自己的后端`server`来充当API的中间层，也需要`'database'(如localStorage, sessionStorage, indexedDB...)`，甚至JS也快支持多线程了。所以现今，绝对不能用语言(别给我说JS只在浏览器内运行)或者某项技术（ESX已经在草案了）来去定义前端。

个人认为，最简单的区别方法就是用API的来源来划分：API的处理者如果在`Node.js`端，那么这个JS项目绝对就是后端（Node端如果只作为中间层转发则不算数）；否则，如果只是作为数据的获取方，并且有UI展现，就算是前端了。

### 2.有无server?

为什么要浪费篇幅去讲前端的定义，就是因为只有明确定义好前端之后，才能解决一个问题：`前端到底要不要server?`

一般的前端项目都会有`dist`产出，通常是由一个`index.html`, 多个`vendor.js`和其他类似图片字体等资源构成。那么这个产出物是如何被render出来的呢？

Case1: webpack server

利用以下webpack配置片段，启用webpack server:

{% highlight javascript %}

var path = require('path');
module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  devServer: {
    contentBase: path.join(__dirname, "dist"),
    compress: true,
    port: 9000,
    watchOptions: {
      poll: true
    }
  }
};

{% endhighlight %}

这种case显然只是render静态的html和资源，因此webpack server只是作为开发，生产则根本不需要。

Case2: 静态服务器

其实和case1类似，如果只是render静态html，那么利用类似`Python`:

{% highlight shell %}

$ python -m SimpleHTTPServer 9999

{% endhighlight %}

或者`Nginx`做代理都是很方便实现的。

Case3: 不必要的Node服务

{% highlight javascript %}

var express = require("express");
var path = require('path');
var project = require('../project.config');

const app = express();
app.use('*', function (req, res) {
  const file = path.resolve(project.basePath, project.outDir, 'index.html');
	res.sendFile(file);
});

app.listen(process.env.PORT || 3000, function () {
  console.log("Listening on port %d!", this.address().port);
});

{% endhighlight %}

如上，只是借用Node服务去render产出物，其实和case2,3没有本质区别。

Case4: API中间层

{% highlight javascript %}

router.route('/articles/send')
  .post(async (req, res) => {
    const {params} = req.body;
    const requestUrl = '/x/x/x/x';
    const response = await requestArticle(requestUrl, params);
    res.status(200).send(response).end();
  });

{% endhighlight %}

这种情况下，Node服务就必须存在，因为很有可能需要用这一层去做身份验证或者有避免随意跨域等安全防护。

### 3.如何部署？

终于到了正题，其实部署无非就是运行环境（server）＋资源（包），因此才需要搞清楚`你的项目到底需不需要server？`，更确切的说是`你的项目的生产环境到底需不需要server？`

对于case1,2,3:

可以选择任意静态服务器，运行在生产环境，每次部署只需拉去最新的代码或生成最新的包。如果需要大量部署，则推荐docker的node或nginx镜像,并将最新的源码打包在内即可。

对于case4:

单机环境建议有部署脚本（如ansible），多机部署则考虑node镜像。BTW，对于case4，如果部署时就是多机的情况，倒不如一劳永逸，开发也是docker。但开发和生产还有点区别，推荐开发时不要把源码打进镜像，毕竟代码总是在变，可以将代码作为`VOLUMN`每次加载上去，然后手动启动，如下片段：

Dockerfile:

{% highlight shell %}

FROM node:7.2

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb http://dl.yarnpkg.com/debian/ stable main" > /etc/apt/sources.list.d/yarn.list
RUN apt-get update && apt-get install yarn

WORKDIR /app

VOLUME /app

{% endhighlight %}

Run docker:

{% highlight shell %}

docker run --rm -v /webpack-demo:/app -ti -p 3000:3000  react-redux-dev:1.0 /bin/bash

{% endhighlight %}

而开发时的Dockerfile则需要打包代码并且设置默认启动命令。

### 4.写在最后

说了这么多，给想实践的朋友推荐以下资源：

- [travis-ci，便捷的github代码构建工具，帮助你进行项目级的持续集成](https://travis-ci.org/)

- [Heroku，完全免费的部署服务，尽管也有时间限制，但是对于快速实践和学习，这是最廉价的方式了](https://dashboard.heroku.com)
