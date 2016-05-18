---
layout: post_layout
title:  "Javascript中apply()的用法"
date:   2015-05-12 10:10PM
categories: jekyll update
type: 2
summary: "Function.prototype.apply()将会调用一个以this和数组形式的arguments为参数的方法，而call()方法则只是在第二个参数形式与之不同而已。"
icon: "js-icon.jpg"
---
### Summary
Function.prototype.apply()将会调用一个以this和数组形式的arguments为参数的方法，而call()方法则只是在第二个参数形式与之不同而已。

{% highlight sh %}
fun.apply(thisArg, [argsArray])
{% endhighlight %}

{% highlight sh %}
fun.call(thisArg, arg1, arg2, arg3...)
{% endhighlight %}

### Description

每当要为一个新的对象添加一个方法时，有时不得不为其重写一个方法。而如果利用apply的话，只需要写一次方法，然后在这个新的对象中继承它即可，十分方便。

apply和call方法十分相似，仅仅只是参数不同而已，但证实因为这一点，我们在用apply时不必知道被调用的对象的具体参数，可以只传递arguments，如此一来，被调用的这个对象将负责handle传递的agrumets.

### Examples

#### 1.Using apply to chain constructors

我们可以像Java那样，利用apply为一个对象创建构造链。在下面的例子中，我们将创建一个名为construct的全局方法，这个方法可以不必让你传递一个一个的参数，取而代之的则是传递一个参数数组。

{% highlight javascript %}
Function.prototype.construct = function (aArgs) {
  var oNew = Object.create(this.prototype);
  this.apply(oNew, aArgs);
  return oNew;
};
{% endhighlight %}

Example usage:

{% highlight javascript %}
function MyConstructor() {
  for (var nProp = 0; nProp < arguments.length; nProp++) {
    this['property' + nProp] = arguments[nProp];
  }
}

var myArray = [4, 'Hello world!', false];
var myInstance = MyConstructor.construct(myArray);

console.log(myInstance.property1);                // 'Hello world!'
console.log(myInstance instanceof MyConstructor); // 'true'
console.log(myInstance.constructor);              // 'MyConstructor'

{% endhighlight %}

#### 2.Using apply and built-in functions

利用apply调用javascript的内建方法是一种比较聪明的用法，例如下面的例子，利用apply调用系统max方法获取最大值。一般如果你要在一个数组中找到最大或最小值，那将不得不进行loop，即使利用系统自带的max函数，那也得传递有限个参数，十分不便。

{% highlight javascript %}
/* min/max number in an array */
var numbers = [5, 6, 2, 3, 7];

/* using Math.min/Math.max apply */
var max = Math.max.apply(null, numbers); /* This about equal to Math.max(numbers[0], ...)
                                            or Math.max(5, 6, ...) */
var min = Math.min.apply(null, numbers);
{% endhighlight %}

#### 3.Using apply in monkey-patching

apply还是一种很好的扩展内置对象方法的方式。如下面的例子，已有someobject.foo方法, 你可以像黑客那样在其原有的基础上修改它，从而破坏其封装性。

{% highlight javascript %}
var someobject = new Object();
someobject.foo = function() {
    console.log('This is the originalfoo function.');
};

var originalfoo = someobject.foo;
someobject.foo = function() {
  // Do stuff before calling function
  console.log(arguments);
  // Call the function as it would have been called normally:
  originalfoo.apply(this,arguments);
  // Run stuff after, here.
}
{% endhighlight %}


