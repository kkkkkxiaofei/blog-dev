---
layout: post_layout
title:  "Element和Node的区别你造吗？"
date:   2016-09-21 12:28PM
categories: jekyll update
type: 2
summary: "我们经常使用document.getElementById去获取DOM中的元素，也会使用childNodes来获取子节点。那么Element和Node区别是什么？什么又是HTMLCollection,HTMLElement,和NodeList呢？"
icon: "js-icon.jpg"
---

### 写在前面

我们经常使用document.getElementById去获取DOM中的元素，也会使用childNodes来获取子节点。那么Element和Node区别是什么？什么又是HTMLCollection,HTMLElement,和NodeList呢？

### 一个简单的页面

{% highlight html %}
<html>
  <body>
    <h1>China</h1>
    <!-- My comment ...  -->
    <p>China is a popular country with...</p>
    <div>
      <button>See details</button>
    </div>
  </body>
</html>

{% endhighlight %}

`body`里的直系子节点一共有三个：`h`,`p`,`div`。对应的我们有`nodeList`可以查看：

{% highlight js %}
document.body.childNodes
{% endhighlight %}

结果:
图图图图图图图图图图图图图图图图图图图图图图图图

问题来了：

- 1.这么会有这么多的#text？
- 2.注释算节点？

在回答上面两个问题之前，就有必要理解下什么是`Node`了。

### Node vs Elemet

以下摘自MDN:

>A Node is an interface from which a number of DOM types inherit, and allows these various types to be treated (or tested) similarly.

>The following interfaces all inherit from Node its methods and properties: Document, Element, CharacterData (which Text, Comment, and CDATASection inherit), ProcessingInstruction, DocumentFragment, DocumentType, Notation, Entity, EntityReference.

简单的说就是`Node`是一个基类，DOM中的`Element`，`Text`和`Comment`都继承于它。
换句话说，`Element`，`Text`和`Comment`是三种特殊的`Node`，它们分别叫做`ELEMENT_NODE`,
`TEXT_NODE`和`COMMENT_NODE`。

***所以我们平时使用的html上的元素，即Element是类型为`ELEMENT_NODE`的`Node`。***

利用`nodeType`可以查看所有类型，如下图：

图图图图图图图图图图图图图图图图图图图图图图图图

到这里，我想我们就可以解释上面两个问题了。

实际上`Node`表示的是DOM树的结构，在html中，节点与节点之间是可以插入文本的，这个插入的空隙就是`TEXT_NODE`，即：

{% highlight html %}
<body>
    we can put text here 1...
    <h1>China</h1>
    we can put text here 2...
    <!-- My comment ...  -->
    we can put text here 3...
    <p>China is a popular country with...</p>
    we can put text here 4...
    <div>
      <button>See details</button>
    </div>
    we can put text here 5 ...
</body>
{% endhighlight %}


这下就顺理成章了，body的直系元素（3）＋ COMMENT_NODE(1) + TEXT_NODE(6) = 9

### NodeList vs HTMLCollection

我们用`document.getElementById`找到了`Node`，又利用childNodes找到了`NodeList`，但我们操作DOM时往往不想操作`Node`(我只想操作元素Element)，那么如何获取ElementList呢？

其实我们经常使用的`getElementsByXXX`返回的就是一个ElementList，只不过它的真实名字是`ElementCollection`。

就像`NodeList`是`Node`的集合一样，`ElementCollection`也是`Element`的集合。但需要特别注意的是：
***NodeList和ElementCollcetion都不是真正的数组***

如果`document.getElementsByTagName('a') instanceof Array`，那么必然是`false`。
