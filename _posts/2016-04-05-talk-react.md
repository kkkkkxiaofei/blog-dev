---
layout: post_layout
title:  "React从零学起"
date:   2016-4-5 21:20PM
categories: jekyll update
type: 2
summary: "初接触React，除了不习惯其组件化的设计原则外，往往它所‘依赖’的众多module也会让初学者感到困惑，使得不知从何学起。此文只是我对React的一些浅析，希望能帮助想要学习React的朋友入门。"
icon: "js-icon.jpg"
---

>初接触React，除了不习惯其组件化的设计原则外，往往它所‘依赖’的众多module也会让初学者感到困惑，使得不知从何学起。此文只是我对React的一些浅析，希望能帮助想要学习React的朋友入门。

### 1.React从来就是独立的

正如上面我提到的，React'依赖'了很多module，但是这种依赖并不是所谓的耦合依赖，只是为了更好的去实现React。换句话说，React只是一个View层面的框架，它可以和其他module自然的融合（更好的去实现）。

我们可以只利用React去实现官网上那个Counter的demo，这里我做了一个简易版。

***index.html***

{% highlight html %}
<!DOCTYPE html>
<html>
  <head>
    <title>Redux counter example</title>
  </head>
  <body>
    <div id="root">
    </div>
    <script src="/static/bundle.js"></script>
  </body>
</html>
{% endhighlight%}

页面只引了一个js文件，该文件为webpack打包而成，具体webpack打包原理不在这里赘述。

***MyCounter.js***

{% highlight js%}
var React = require('react');

var Counter = React.createClass({
    getInitialState: function() {
        return {value: 0};
    },
    plus: function() {
        this.setState({
            value: ++this.state.value
        });
    },
    minus: function() {
        this.setState({
            value: --this.state.value
        });
    },
    render: function() {
        return (
            <div>
                <button onClick={this.plus}>+</button>
                <span>{this.state.value}</span>
                <button onClick={this.minus}>-</button>
            </div>
        );
    }
});

module.exports = Counter;
{% endhighlight %}

这是典型的React的Component，它的内部实现了计数的算法以及state的管理机制，这个Component的实例就是计数器，该计数器也很简单，可以实现增加和减少。

最后是 ***index.js***

{% highlight js %}
var React = require('react');
var ReactDOM = require('react-dom');
var MyCounter = require('./components/MyCounter');

ReactDOM.render(
    <MyCounter />,
    document.getElementById('root')
);
{% endhighlight %}

到此，我们就利用React实现了一个小小的计数器，尽管它很简单，但是却未使用任何其他module，所以我在上面提到，React本身就是独立的。

所以，引用其他module，只是为了实现更复杂的React App，使得其更具有扩展性。

### 2.Container

那么问题来了，如果我还像要一个类似的Component，但是每次计数的时候不是加1或者减1，而是乘2或除2，那怎么做呢？

你可别告诉我重新一个类似上面MyCounter的Component，然后绑定不同的事件，那如果是这样的话React也太low了吧，这还叫什么组件化呢，组件化最基本的特点就是复用啊。

所以React期望我们这么做：

对于任何Compoent，尽量将其作为静态展示的Component，即其只负责展示UI，然后在它的外层嵌套一个Container， Container中定义了该Compoent所需要的参数以及方法，这样，当我们需要复用Component时，UI已经是现成的了，而Container中的逻辑部分也可以共享，换个“壳子”就是一个具有其他功能的Compoent了。

于是，分解如下：

***MyCounterContainer.js***

{% highlight js %}
var React = require('react');
var Counter = require('../components/MyCounter');

var CounterContainer = React.createClass({
    getInitialState: function() {
        return {
            value: this.props.value
        };
    },
    plus: function() {
        this.setState({
            value: this.state.value + 1
        });
    },
    minus: function() {
        this.setState({
            value: this.state.value - 1
        });
    },
    render: function() {
        return (
            <Counter plus={this.plus}
                     minus={this.minus}
                     value={this.state.value} />
        );
    }
});

module.exports = CounterContainer;
{% endhighlight %}

***MyCounter***

{% highlight js %}
var React = require('react');

var Counter = React.createClass({
    render: function() {
        return (
            <div>
                <span>{this.props.value}</span>
                <button onClick={this.props.plus}>+</button>
                <button onClick={this.props.minus}>-</button>
            </div>
        );
    }
});

module.exports = Counter;
{% endhighlight %}

***index.js***

{% highlight js %}
var React = require('react');
var ReactDOM = require('react-dom');
var MyCounterContainer = require('./container/MyCounterContainer');

ReactDOM.render(
    <MyCounterContainer value={0} />,
    document.getElementById('root')
);
{% endhighlight %}

UI与逻辑分离成功，是不是感觉瞬间清爽许多。
关于什么时Contianer Component和Presentation Component，推荐[此文](https://medium.com/@learnreact/container-components-c0e67432e005#.w2cmqjegm)。

### 3.Use store to help dispatch actions

分离了UI后，的确逻辑上清楚了许多，但仔细观察会发现，上面的 ***MyCounterContainer*** 状态的改变只是两个button。而React认为Component的状态变化必定是由一个行为，即action造成的，因此，我们需要将上面的加减法抽象为一个行为驱动的事件，即一个行为对应一种状态结果。而 ***redux*** 就是干这事儿的，它通过createStore去创建一个store，这个store可以管理和知晓这个Component的状态，它通过dispatch分发action然后得到最新的状态结果。

利用store，我们将 ***MyCounterContainer*** 重构如下：

{% highlight js %}
var React = require('react');
var Counter = require('../components/MyCounter');
var createStore = require('redux').createStore;


var counter = function(state, action) {
    switch (action.type) {
        case 'PLUS': 
            return state + action.value;
        case 'MINUS': 
            return state - action.value;
        default: 
            return state;
    }
};

var store = createStore(counter, 1000);
var CounterContainer = React.createClass({
    plus: function() {
        var nextState = store.dispatch({
            type: "PLUS",
            value: 2
        });
        this.setState(nextState);
    },
    minus: function() {
        var nextState = store.dispatch({
            type: "MINUS",
            value: 2
        });
        this.setState(nextState);
    },
    render: function() {
        return (
            <Counter plus={this.plus} 
                     minus={this.minus}
                     value={store.getState()} />
        );
    }
});

module.exports = CounterContainer;
{% endhighlight %}

### 4.Use connect to manage the dispatch and reducer

仔细观察上次重构，不难发现还是有些问题：

其一，尽管利用store帮我们管理了state，但是还是得我们手动setState，太过耦合。

其二，对于传递给子Component的参数，还是写死在Container里，不具有封装性和灵活性。

为了解决这个问题，我们可以利用 ***react-redux*** 的connect来解决。
connect可以把自定义的state和dispatch分发事件绑定到Component上，其中mapStateToProps正如其名，可以将state作为Component的props传递下去；而mapDispatchToProps则可以把action触发逻辑传递下去，这样我们可以很灵活的传递功能事件了。

利用connect我们继续重构，***MyCounterContainer*** 如下：

{% highlight js%}
var React = require('react');
var Counter = require('../components/MyCounter');
var createStore = require('redux').createStore;
var connect = require('react-redux').connect;

var mapStateToProps = function(state) {
    return {
        value: state
    }
};

var mapDispatchToProps = function(dispatch) {
    return {
        plus: function() {
            dispatch({
                type: "PLUS", 
                value: 2
            });
        },
        minus: function() {
            dispatch({
                type: "MINUS", 
                value: 2
            });
        }
    }
};

var CounterContainer = connect(
    mapStateToProps,
    mapDispatchToProps
)(Counter);

module.exports = CounterContainer;
{% endhighlight %}

### 5.Split actions

上面的例子已经很接近React的初级App的设计了，但当我们的Component特别复杂时，往往action也会难抽象，像上面的dispatch({type: "PLUS", value: 2});偶合度太高，因为我们根本不知道这个action为什么是这样，就好比我们随便写了一个常量而并未定义任何变量名一样，别人是很难阅读的。因此比较好的做法是把action更小的分离，比如上面的action，我们可以分离成如下：

{% highlight js %}
module.exports.plusAction = function(val) {
    return {
        type: "PLUS",
        value: val
    };
}

module.exports.minusAction = function(val) {
    return {
        type: "MINUS",
        value: val
    };
}
{% endhighlight %}

这样，在dispatch时，也会显得很简洁。

### 6.ES6 refactor
ES6部分就不在这里赘述了，大多都是语法问题，建议大家可以参考[阮老师的书ES6入门](http://es6.ruanyifeng.com/#docs/function)

### 7.写在最后

个人认为，学习React十分不推荐一上手就用各种module，或者照猫画虎式的去填空，这样只能是到头来什么也不会。当你从头开始去理解时，才能找到痛点，而当你有痛点时你才需要重构，那么此时可能某个module就是你想要的。你用它只是为了省时，而不是你做不出来才用它。借用我前几天在知乎上回答的问题“用库丢脸不？”，我的观点是：***用库不丢脸，不懂库还非要用库才丢脸***。