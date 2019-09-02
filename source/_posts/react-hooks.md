---
title: React之Hooks
date: 2019-09-01 16:30:40
tags: 
   - React
   - Hooks
categories: Front-End
---

## 一、一个最简单的Hooks

> 首先让我们看一下一个简单的有状态组件

```jsx
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```

> 我们再来看一下使用hooks后的版本

```jsx
import { useState } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

> 是不是简单多了！可以看到，Example变成了一个函数，但这个函数却有自己的状态（count），同时它还可以更新自己的状态（`setCount`）。这个函数之所以这么了不得，就是因为它注入了一个`hook--useState`，就是这个hook让我们的函数变成了一个有状态的函数

除了`useState`这个`hook`外，还有很多别的`hook`，比如`useEffect`提供了类似于`componentDidMount`等生命周期钩子的功能，`useContext`提供了上下文（`context`）的功能等等

> `Hooks`本质上就是一类特殊的函数，它们可以为你的函数型组件（`function component`）注入一些特殊的功能。咦？这听起来有点像被诟病的`Mixins`啊？难道是`Mixins`要在`react`中死灰复燃了吗？当然不会了，等会我们再来谈两者的区别。总而言之，这些`hooks`的目标就是让你不再写`class`，让`function`一统江湖

## 二、React为什么要搞一个Hooks

**想要复用一个有状态的组件太麻烦了**

> 我们都知道react都核心思想就是，将一个页面拆成一堆独立的，可复用的组件，并且用自上而下的单向数据流的形式将这些组件串联起来。但假如你在大型的工作项目中用react，你会发现你的项目中实际上很多react组件冗长且难以复用。尤其是那些写成class的组件，它们本身包含了状态（state），所以复用这类组件就变得很麻烦

- 那之前，官方推荐怎么解决这个问题呢？答案是：渲染属性（`Render Props`）和高阶组件（`Higher-Order Components`）。我们可以稍微跑下题简单看一下这两种模式

> 渲染属性指的是使用一个值为函数的`prop`来传递需要动态渲染的`nodes`或组件。如下面的代码可以看到我们的`DataProvider`组件包含了所有跟状态相关的代码，而`Cat`组件则可以是一个单纯的展示型组件，这样一来`DataProvider`就可以单独复用了

```jsx
import Cat from 'components/cat'
class DataProvider extends React.Component {
  constructor(props) {
    super(props);
    this.state = { target: 'Zac' };
  }

  render() {
    return (
      <div>
        {this.props.render(this.state)}
      </div>
    )
  }
}

<DataProvider render={data => (
  <Cat target={data.target} />
)}/>
```

> 虽然这个模式叫`Render Props`，但不是说非用一个叫`render`的`props`不可，习惯上大家更常写成下面这种

```
...
<DataProvider>
  {data => (
    <Cat target={data.target} />
  )}
</DataProvider>
```

> 高阶组件这个概念就更好理解了，说白了就是一个函数接受一个组件作为参数，经过一系列加工后，最后返回一个新的组件。看下面的代码示例，`withUser`函数就是一个高阶组件，它返回了一个新的组件，这个组件具有了它提供的获取用户信息的功能

```jsx
const withUser = WrappedComponent => {
  const user = sessionStorage.getItem("user");
  return props => <WrappedComponent user={user} {...props} />;
};

const UserPage = props => (
  <div class="user-container">
    <p>My name is {props.user}!</p>
  </div>
);

export default withUser(UserPage);
```

> 以上这两种模式看上去都挺不错的，很多库也运用了这种模式，比如我们常用的React Router。但我们仔细看这两种模式，会发现它们会增加我们代码的层级关系。最直观的体现，打开devtool看看你的组件层级嵌套是不是很夸张吧。这时候再回过头看我们上一节给出的hooks例子，是不是简洁多了，没有多余的层级嵌套。把各种想要的功能写成一个一个可复用的自定义hook，当你的组件想用什么功能时，直接在组件里调用这个hook即可

![](https://user-gold-cdn.xitu.io/2018/11/9/166f7bc8480d53c6)

## 三、生命周期钩子函数里的逻辑太乱了

> 我们通常希望一个函数只做一件事情，但我们的生命周期钩子函数里通常同时做了很多事情。比如我们需要在`componentDidMount`中发起`ajax`请求获取数据，绑定一些事件监听等等。同时，有时候我们还需要在`componentDidUpdate`做一遍同样的事情。当项目变复杂后，这一块的代码也变得不那么直观

## 四、classes真的太让人困惑了

> 我们用`class`来创建`react`组件时，还有一件很麻烦的事情，就是`this`的指向问题。为了保证this的指向正确，我们要经常写这样的代码：`this.handleClick = this.handleClick.bind(this)`，或者是这样的代码：`<button onClick={() => this.handleClick(e)}>`。一旦我们不小心忘了绑定`this`，各种bug就随之而来，很麻烦

还有一件让我很苦恼的事情。我在之前的react系列文章当中曾经说过，尽可能把你的组件写成无状态组件的形式，因为它们更方便复用，可独立测试。然而很多时候，我们用function写了一个简洁完美的无状态组件，后来因为需求变动这个组件必须得有自己的`state`，我们又得很麻烦的把`function`改成`class`


**在这样的背景下，Hooks便横空出世了**

## 五、什么是State Hooks？

> 回到一开始我们用的例子，我们分解来看到底state hooks做了什么

```js
import { useState } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

**声明一个状态变量**

```js
import { useState } from 'react';

function Example() {
  const [count, setCount] = useState(0);
}
```

> `useState`是`react`自带的一个`hook`函数，它的作用就是用来声明状态变量。`useState`这个函数接收的参数是我们的状态初始值（`initial state`），它返回了一个数组，这个数组的第[0]项是当前当前的状态值，第[1]项是可以改变状态值的方法函数

- 所以我们做的事情其实就是，声明了一个状态变量`count`，把它的初始值设为0，同时提供了一个可以更改`count`的函数`setCount`

**读取状态值**

```
<p>You clicked {count} times</p>
```

> 是不是超简单？因为我们的状态count就是一个单纯的变量而已，我们再也不需要写成`{this.state.count}`这样了

**更新状态**

```
 <button onClick={() => setCount(count + 1)}>
    Click me
  </button>
```

> 当用户点击按钮时，我们调用setCount函数，这个函数接收的参数是修改过的新状态值。接下来的事情就交给`react`了，react将会重新渲染我们的Example组件，并且使用的是更新后的新的状态，即`count=1`。

## 六、什么是Effect Hooks?

```js
import { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  // 类似于componentDidMount 和 componentDidUpdate:
  useEffect(() => {
    // 更新文档的标题
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

我们对比着看一下，如果没有hooks，我们会怎么写？

```js
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  componentDidMount() {
    document.title = `You clicked ${this.state.count} times`;
  }

  componentDidUpdate() {
    document.title = `You clicked ${this.state.count} times`;
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```

1. 我们写的有状态组件，通常会产生很多的副作用（`side effect`），比如发起`ajax`请求获取数据，添加一些监听的注册和取消注册，手动修改dom等等。我们之前都把这些副作用的函数写在生命周期函数钩子里，比如`componentDidMount`，`componentDidUpdate`和`componentWillUnmount`。而现在的useEffect就相当与这些声明周期函数钩子的集合体。它以一抵三
2. 同时，由于前文所说hooks可以反复多次使用，相互独立。所以我们合理的做法是，给每一个副作用一个单独的useEffect钩子。这样一来，这些副作用不再一股脑堆在生命周期钩子里，代码变得更加清晰


> 更多参考 http://www.ruanyifeng.com/blog/2019/09/react-hooks.html
