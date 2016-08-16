---
layout: post_layout
title:  "\"Remote Work Via Git\"的正确打开方式"
date:   2016-08-16 19:09PM
categories: jekyll update
type: 2
summary: "git pull -r origin branch是程序员最常用的Git指令之一了。可现今为了保证安全性，许多公司的Git服务器并不像访问Github那样容易，往往得通过VPN才能进行访问。那么问题来了，假如你在家里办公，连不上VPN肿么办？别懵逼，往下看。"
icon: "thinking-icon.jpg"
---

### 写在前面

`git pull -r origin branch`是程序员最常用的`Git`指令之一了。可现今为了保证安全性，许多公司的`Git Server`并不像访问`Github`那样容易，往往得通过`VPN`才能进行访问。那么问题来了，假如你在家里办公，连不上`VPN`肿么办？别懵逼，往下看。

### 场景一

小王，小宋和小马都是同一个公司的程序员，它们都在同一个项目上工作，本地都有同一个`Git Repository`。今天轮到小宋和小马结对编程了，小宋在家，小马和小王在公司。由于只能通过客户提供的`VPN`，并且在公司的网络下进行连接才能pull到代码，所以小宋无法获取最新的代码，十分沮丧。这时，小马安慰小宋说：“我可以搭建一个本地`Git Server`让你来pull代码。”

#### 1.首先，小马查看了自己的IP地址
{% highlight sh %}
ifconfig
{% endhighlight %}

#### 2.小马让小宋在家里ping一下自己的IP
{% highlight sh %}
ping xx.xx.xx.xx
{% endhighlight %} 

**⚠良心提示：如果ping不通，就关闭此页面，洗洗睡吧**

#### 3.小马搭建一个本地Git服务器
小马有一个名叫`xinjibiao`的`Repository`，在`/project/xinjibiao`路径下。
由于小马可以连接上客户的`VPN`，所以她先pull了下代码，确保自己的代码是最新的，然后进入`project`目录下后：

{% highlight sh %}
git daemon --base-path=. --export-all --reuseaddr --informative-errors --verbose
{% endhighlight %}

如此小马就将自己的本地`Git Server`建起来了，并且以当前路径`/project`作为`localhost`

#### 4.小宋可以pull小马的代码啦
因为小宋的`remote`只有`origin`，为了让小马的`Git Server`作为自己的新`remote`，小宋需要：

{% highlight sh %}
git add remote xiaoma git://xx.xx.xx.xx/xinjibiao
{% endhighlight %}

其中`xx.xx.xx.xx`为小马的IP，`xiaoma`为新的`remote`的别名，如此，小宋就可以把小马的电脑当作`Git Repository`的服务器，开始pull代码啦：

{% highlight sh %}
git pull -r xiaoma master
{% endhighlight %}

此时小宋在自己`master`分支上成功的pull到了小马`master`上的代码，也就是最新的代码，小宋很开心，呵呵。

### 场景二

小宋成功pull到最新代码后，在自己的电脑上与小马结对编程完成了3个commit需要提交，这时才发现，自己是连不上`VPN`的，代码都是从小宋那里pull的，更何谈去push代码了。这时，聪明的小马又给小宋出了高招：“你可以把你的提交在本地做成patch文件，然后发给小王，他就能帮你push了。”

于是在小宋的指导下，小马找到了小王。

#### 1.首先，小马在本地将自己的3个commit打成patch文件

{% highlight sh %}
git format-patch -3 --stdout > wo_he_xiaoma_do_something.patch
{% endhighlight %}

指令很简单，小马将HEAD中的前3个commit的changes放入一个名为`wo_he_xiaoma_do_something`的新的patch文件里。

#### 2.小宋把patch文件发送给了小王

{% highlight sh %}
git apply --stat wo_he_xiaoma_do_something.patch
{% endhighlight %}

这样小王就将小马和小宋写的代码放入到了自己的`Reposity`中，并且push到了远端。

**⚠良心提示：－－check 可以检查文件名**

### 写在最后

其实，本文主要讲了两个常用的Git操作：在无法连接国外`Git Server`的时候如何pull别人的代码和push自己的代码，相信大家看完本教程一定能有所收获。