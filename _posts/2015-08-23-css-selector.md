---
layout: post_layout
title:  "你必须记住的30个CSS选择器[译]"
date:   2015-08-23 20:12PM
categories: jekyll update
type: 2
summary: "有30个CSS选择器你必须烂熟于心，它们适应于当今各大主流浏览器。"
icon: "css3-icon.jpg"
---
#### 开篇

>有30个CSS选择器你必须烂熟于心，它们适应于当今各大主流浏览器。

#### 1.`*`

{% highlight css %}
* {
  margin: 0;
  padding: 0;
}
{% endhighlight %}

`*`选择器选择的是每一个单一元素。很多程序员用上面的CSS将所有元素的margin和padding清零。虽然这是有效的，但最好还是别这么做，这会使得浏览器的负担很重。

`*`选择器也可以用在孩子选择器中。

{% highlight css %}
#container * {
  border: 1px solid black;
}
{% endhighlight %}

这会使#container所有孩子都有border，但还是那句话，如果不是必须得这么做，还是别用星选择器。

[view demo](http://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/selectors/star.html)

#### Compatibility
* IE6+
* Firefox
* Chrome
* Safari
* Opera


#### 2.`#x`

{% highlight css %}
#container {
   width: 960px;
   margin: auto;
}
{% endhighlight %}

id选择器的优先级很高，因此在用之前问问自己：`我仅仅是为了找到这个元素而去给他加id的吗？`

[view demo](http://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/selectors/id.html)

#### Compatibility
* IE6+
* Firefox
* Chrome
* Safari
* Opera

####  3.`.x`

{% highlight css %}
.error {
  color: red;
}
{% endhighlight %}

class选择器和id选择器不同，首先优先级没有id高，再者id选择器只能选择一个，而class选择器则可以筛选出来一组。

[view demo](http://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/selectors/class.html)

#### Compatibility
* IE6+
* Firefox
* Chrome
* Safari
* Opera

#### 4.`x y`

{% highlight css %}
li a {
  text-decoration: none;
}
{% endhighlight %}

当不是选择所有后代时，后代选择器将会非常有用，但有一点得注意：
`如果你的选择器是x y z a b.error, 那你就用错了。你得问问自己是否需要应用每一层？`

[view demo](http://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/selectors/descend.html)

#### Compatibility
* IE6+
* Firefox
* Chrome
* Safari
* Opera

#### 5.`x`

{% highlight css %}
a { color: red; }
ul { margin-left: 0; }
{% endhighlight %}

如果想选择同一类元素，那就不要用id或class了，直接用元素选择器。

[view demo](http://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/selectors/tagName.html)

#### Compatibility
* IE6+
* Firefox
* Chrome
* Safari
* Opera


#### 6.`x:visted and x:link`

{% highlight css %}
a:link { color: red; }
a:visted { color: purple; }
{% endhighlight %}

我们常常用伪类:link筛选a标签是还未被点击；而用:visited去筛选哪些别点击过了。

[view demo](http://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/selectors/links.html)

#### Compatibility
* IE7+
* Firefox
* Chrome
* Safari
* Opera


#### 7.`x + y`

{% highlight css %}
ul + p {
   color: red;
}
{% endhighlight %}

相邻选择器会选择第一个相邻的元素，如上面的例子会让ul后第一个段落的字体变为红色（而ul与p之间是不能有其他元素的）。

[view demo](http://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/selectors/adjacent.html)

#### Compatibility
* IE7+
* Firefox
* Chrome
* Safari
* Opera


#### 8.`x > y`

{% highlight css %}
div#container > ul {
  border: 1px solid black;
}
{% endhighlight %}

这也是一种后代选择器，但它与`x y`这种后代选择器不同，它只能选择直系后代。如：

{% highlight html %}
<div id="container">
   <ul>
      <li> List Item
        <ul>
           <li> Child </li>
        </ul>
      </li>
      <li> List Item </li>
      <li> List Item </li>
      <li> List Item </li>
   </ul>
</div>
{% endhighlight %}

在这个例子中，`#cotainer > ul`只会选择第一个ul,而不会search到ul里面那个包含li的ul。

[view demo](http://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/selectors/childcombinator.html)

#### Compatibility
* IE7+
* Firefox
* Chrome
* Safari
* Opera


#### 9.`x ~ y`

{% highlight css %}
ul ~ p {
   color: red;
}
{% endhighlight %}

这种兄弟选择器与`x + y`类似，但不同的是，后者只能筛选第一个p，而这种却可以满足ul下所有的直系p标签。

[view demo](http://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/selectors/generalcombinator.html)

#### Compatibility
* IE7+
* Firefox
* Chrome
* Safari
* Opera


#### 10.`x[title]`

{% highlight css %}
a[title] {
   color: green;
}
{% endhighlight %}

属性选择器。这将选择所有具有title属性的a标签。

[view demo](http://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/selectors/attributes.html)

#### Compatibility
* IE7+
* Firefox
* Chrome
* Safari
* Opera


#### 11.`x[href="foo"]`

{% highlight css %}
a[href="http://net.tutsplus.com"] {
  color: #1f6053; /* nettuts green */
}
{% endhighlight %}

这个选择器可以选择链接为`href="http://net.tutsplus.com"`的a标签，可如果这个里这个链接变了呢？，这未免有些严格，可以适当的用正则表达式去匹配。

[view demo](http://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/selectors/attributes2.html)

#### Compatibility
* IE7+
* Firefox
* Chrome
* Safari
* Opera


#### 12.`x[href*="nettuts"]`

{% highlight css %}
a[href*="tuts"] {
  color: #1f6053; /* nettuts green */
}
{% endhighlight %}

'*'号将匹配href中含有nuttuts字符，如果想更加严格，还可以用`^`和`$`表示开始和结束。

[view demo](http://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/selectors/attributes3.html)

#### Compatibility
* IE7+
* Firefox
* Chrome
* Safari
* Opera


#### 13.`x[href^="http"]`

{% highlight css %}
a[href^="http"] {
   background: url(path/to/external/icon.png) no-repeat;
   padding-left: 10px;
}
{% endhighlight %}

这样去筛选具有有效href的a将匹配http://和https://.

[view demo](http://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/selectors/attributes4.html)

#### Compatibility
* IE7+
* Firefox
* Chrome
* Safari
* Opera


#### 14.`x[href$=".jpg"]`

{% highlight css %}
a[href$=".jpg"] {
   color: red;
}
{% endhighlight %}

这将会选择链接为jpg格式的图片链接，可是如果图片类型为png或gif等怎么办？

[view demo](http://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/selectors/attributes5.html)

#### Compatibility
* IE7+
* Firefox
* Chrome
* Safari
* Opera


#### 15.`x[data-*="foo"]`

{% highlight css %}
a[data-filetype="image"] {
   color: red;
}
{% endhighlight %}

按照规则`14.`我们可能得：

{% highlight css %}
a[href$=".jpg"],
a[href$=".jpeg"],
a[href$=".png"],
a[href$=".gif"] {
   color: red;
}
{% endhighlight %}

可这也太。。。
我们可以加一个属性用以标示。

{% highlight html %}
<a href="path/to/image.jpg" data-filetype="image"> Image Link </a>
{% endhighlight %}

{% highlight css %}
a[data-filetype="image"] {
   color: red;
}
{% endhighlight %}


[view demo](http://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/selectors/attributes6.html)

#### Compatibility
* IE7+
* Firefox
* Chrome
* Safari
* Opera


#### 16.`x[foo~="bar"]`

{% highlight css %}
a[data-info~="external"] {
   color: red;
}
 
a[data-info~="image"] {
   border: 1px solid black;
}
{% endhighlight %}

`~`将会让我们匹配到属性值被空格分隔符分开的值，如：

{% highlight css %}
"<a href="path/to/image.jpg" data-info="external image"> Click Me, Fool </a>
{% endhighlight %}

[view demo](http://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/selectors/attributes7.html)

#### Compatibility
* IE7+
* Firefox
* Chrome
* Safari
* Opera


#### 17.`x:checked`

{% highlight css %}
input[type=radio]:checked {
   border: 1px solid black;
}
{% endhighlight %}

这个常常对checkbox非常有用。

[view demo](http://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/selectors/checked.html)

#### Compatibility
* IE9+
* Firefox
* Chrome
* Safari
* Opera


#### 18.`x:after`

伪类`before`和`after`已经有了一些新的用法，比如最常见的：
{% highlight css %}
.clearfix:after {
    content: "";
    display: block;
    clear: both;
    visibility: hidden;
    font-size: 0;
    height: 0;
    }
 
.clearfix { 
   *display: inline-block; 
   _height: 1%;
}
{% endhighlight %}

没错，这就是默认标准clearfix的实现原理。

#### Compatibility
* IE8+
* Firefox
* Chrome
* Safari
* Opera


#### 19.`x:hover`

{% highlight css %}
div:hover {
  background: #e3e3e3;
}
{% endhighlight %}

但是得注意，`:hover`在早期IE中并不适用。

#### Compatibility
* IE6+（In IE6, :hover must be applied to an anchor element）
* Firefox
* Chrome
* Safari
* Opera

#### 20.`x:not(selector)`

{% highlight css %}
div:not(#container) {
   color: blue;
}
{% endhighlight %}

反选选择器。

[view demo](http://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/selectors/not.html)

#### Compatibility
* IE9+
* Firefox
* Chrome
* Safari
* Opera


#### 21.`x::pseudoElement`

{% highlight css %}
p::first-line {
   font-weight: bold;
   font-size: 1.2em;
}

p::first-letter {
   float: left;
   font-size: 2em;
   font-weight: bold;
   font-family: cursive;
   padding-right: 2px;
}
{% endhighlight %}

伪元素选择器，注意尽量还是按标准来，多使用`::`而不是`:`。


[view demo](http://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/selectors/pseudoElements.html)

#### Compatibility
* IE6+
* Firefox
* Chrome
* Safari
* Opera


#### 22.`x:nth-child(n)`

{% highlight css %}
li:nth-child(3) {
   color: red;
}
{% endhighlight %}

选择一组中特定的孩子。n表示第几个，也可以是表达式，如2n+1,2n.

[view demo](http://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/selectors/nth.html)

#### Compatibility
* IE9+
* Firefox 3.5+
* Chrome
* Safari
* Opera


#### 23.`x:nth-last-child(n)`

{% highlight css %}
li:nth-last-child(2) {
   color: red;
}
{% endhighlight %}

如果li有400个，而你需要target到第397个，那用这个咱合适不过了。

[view demo](http://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/selectors/nthLast.html)

#### Compatibility
* IE9+
* Firefox 3.5+
* Chrome
* Safari
* Opera


#### 24.`x:nth-of-type(n)`

{% highlight css %}
ul:nth-of-type(3) {
   border: 1px solid black;
}
{% endhighlight %}

如果ul没有id，而你又要找第三个ul，那个这种方式是不错。

[view demo](http://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/selectors/nthOfType.html)

#### Compatibility
* IE9+
* Firefox 3.5+
* Chrome
* Safari
* Opera


#### 25.`x:nth-last-of-type(n)`

{% highlight css %}
ul:nth-last-of-type(3) {
   border: 1px solid black;
}
{% endhighlight %}

与`ul:nth-of-type`刚好相反。


#### Compatibility
* IE9+
* Firefox 3.5+
* Chrome
* Safari
* Opera


#### 26.`x:first-child`

{% highlight css %}
ul li:first-child {
   border-top: none;
}
{% endhighlight %}

[view demo](http://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/selectors/firstChild.html)

#### Compatibility
* IE7+
* Firefox
* Chrome
* Safari
* Opera


#### 27.`x:last-child`

##### Example

{% highlight html %}
<ul>
   <li> List Item </li>
   <li> List Item </li>
   <li> List Item </li>
</ul>
{% endhighlight %}

{% highlight css %}
ul {
 width: 200px;
 background: #292929;
 color: white;
 list-style: none;
 padding-left: 0;
}
 
li {
 padding: 10px;
 border-bottom: 1px solid black;
 border-top: 1px solid #3c3c3c;
}
{% endhighlight %}

![](/../img/selector/list.png)

[view demo](https://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/extraBorders.png)

但是我不想要第一个的上边框和最后一个的下边框，可以这么做：
{% highlight css %}
li:first-child {
    border-top: none;
}
 
li:last-child {
   border-bottom: none;
}
{% endhighlight %}

[view demo](http://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/selectors/firstChild.html)

#### Compatibility
* IE9+
* Firefox
* Chrome
* Safari
* Opera


#### 28.`X:only-child`

{% highlight css %}
div p:only-child {
   color: red;
}
{% endhighlight %}

这将匹配div里只有一个p的元素。如：

{% highlight css %}
<div><p> My paragraph here. </p></div>
 
<div>
   <p> Two paragraphs total. </p>
   <p> Two paragraphs total. </p>
</div>
{% endhighlight %}

[view demo](http://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/selectors/onlyChild.html)

#### Compatibility
* IE9+
* Firefox
* Chrome
* Safari
* Opera


#### 29.`X:only-of-type`

{% highlight css %}
li:only-of-type {
   font-weight: bold;
}
{% endhighlight %}

这将匹配元素内只有一个li的元素，有时这个做法很简便。比如要寻找只有一个列表的ul，可以：


{% highlight css %}
ul > li:only-of-type {
   font-weight: bold;
}
{% endhighlight %}

[view demo](http://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/selectors/onlyOfType.html)

#### Compatibility
* IE9+
* Firefox 3.5+
* Chrome
* Safari
* Opera


#### 30.`x:first-of-type`

##### Example

{% highlight html %}
<div>
   <p> My paragraph here. </p>
   <ul>
      <li> List Item 1 </li>
      <li> List Item 2 </li>
   </ul>
 
   <ul>
      <li> List Item 3 </li>
      <li> List Item 4 </li>
   </ul>   
</div>
{% endhighlight %}

如何寻找上面的 "List Item 2"呢？

##### 办法1

{% highlight html %}
ul:first-of-type > li:nth-child(2) {
   font-weight: bold;
}
{% endhighlight %}

##### 办法2

{% highlight html %}
p + ul li:last-child {
   font-weight: bold;
}
{% endhighlight %}

##### 办法3

{% highlight html %}
ul:first-of-type li:nth-last-child(1) {
   font-weight: bold;   
}
{% endhighlight %}

[view demo](http://cdn.tutsplus.com/net/uploads/legacy/840_cssSelectors/selectors/firstOfType.html)

#### Compatibility
* IE9+
* Firefox 3.5+
* Chrome
* Safari
* Opera

#### 总结

上述选择器在IE6中的使用要尤其小心，但是别因为这而影响你阅读这篇文章。你可以参考一下[浏览器兼容表](http://www.quirksmode.org/css/contents.html)，或者你可以用[ Dean Edward's excellent IE9.js script ](http://code.google.com/p/ie7-js/)为旧版浏览器提供这些选择器的支持。

另外，当你在使用一些Javascript库时，如jQuery，请尽量的使用这些原生的CSS3选择器，因为浏览器选择器引擎将会按照浏览器的原生方式去解析，[这将使得你的代码更加高效](http://jsperf.com/jquery-css3-not-vs-not)。


`注：此文为译文`[原文请戳](http://www.sitepoint.com/understanding-block-formatting-contexts-in-css/)

