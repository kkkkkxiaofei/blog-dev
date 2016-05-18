---
layout: post_layout
title:  "Collapsing margins(外边距合并)"
date:   2015-07-26 22:14PM
categories: jekyll update
type: 2
summary: "块元素的margin-top或margin-bottom
有时会合并为一个margin，这种情况称之为外边距的合并，即collapsing margins."
icon: "css3-icon.jpg"
---
#### 开篇

>块元素的上边距或下边距有时会合并为一个margin，这种情况称之为外边距的合并，即collapsing margins.

#### 一个常见的css样式的bug

html&css：

{% highlight css %}
<!DOCTYPE html>
<html>
    <style type="text/css">
        html, body{
            width: 100%;
            height: 100%;
            padding: 0;
            margin: 0;
        }
        #main {
            width: 200px;
            height: 200px;
            background: yellow;
        }
        #sub {
            color: #fff;
            width: 200px;
            padding: 0;
            margin: 0;
            margin-top: 20px;
            background: black;
        }
    </style>
<body>
    <div id="main">
        <div id="sub">this is sub block</div>
    </div>
</body>
</html>
{% endhighlight %}

效果：

> ![](/../img/collapsing/collapsing.png)

上图中父div包裹着子div，css的样式很明显想让子div与父容器有20px的margin－top，可是很不幸，父div和子div上边距重合了，而且莫名其妙的与body有了20px的上边距（body为白色背景区域）。

这个bug很常见，也经常被大家忽略，但是它却揭示了一个很重要的概念，即`外边距合并（Collapsing margins）`,想要彻底解决这个问题（当然了，你随便在chrome里修修改改也可以调好，但你懂真正的原因吗？），还是得看[W3C的官方规范](http://www.w3.org/TR/CSS2/box.html#collapsing-margins),为了易于阅读，我截取了一段源自[MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin_collapsing)的翻译：

#### 外边距合并发生在下面三种基本情形:

* `1.毗邻元素Adjacent siblings`
毗邻元素的外边距会合并（当靠后的元素 清除浮动 时除外)。

* `2.毗邻元素Adjacent siblings`
如果块元素的 margin-top 与它的第一个子元素之间没有border, padding, inline content, 或 clearance 分隔，或者块元素的 margin-bottom 与它的最后一个子元素之间没有padding, inline content, height, min-height, or max-height 分隔，那么外边距会合并。

* `3.空块元素`
如果块元素 margin-top 与 margin-bottom 之间没有border, padding, inline content, height, 与min-height来分隔， 那么它的上下外边距合并。

可以看到，上面的bug属于情况2，即只要为主div设置padding即可解决(或border等，视具体情况而论)。




















