# Hooks
> Hooks是React在React v16.7.0-alpha 版本发布的新API，它能够让你在不使用`class`定义组件的同时使用`state`以及其他React功能
话不多说，先来看一个例子
```js
import { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
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
> 这个官方给的一个例子，在这里直接拿来用了

这个Example组件的功能主要是在用户按下按钮的时候，上方显示的数字增加1（默认为0）

接下来对比一下class版本相同功能的组件，请看例子
```js
import React, { Component } from 'react'

class Example extends Component {
  constructor(props) {
    super(props)
    this.state = {
      count: 0
    }
    this.handleOnClick = this.handleOnClick.bind(this)
  }

  handleOnClick() {
    this.setState({count: count + 1})
  }

  render() {
    const { count } = this.state
    return (
      <div>
        <p>You clicked {count} times</p>
        <button onClick={this.handleOnClick}>
          Click me
        </button>
      </div>
    )
  }
}
```
在这里，先不详细介绍Hooks怎么用，而是先对比了两个版本的代码，可以看出：
- Hooks版本的代码更加简洁，没有冗长的constructor初始化代码
- Hooks版本的onClick处理函数没有this绑定的问题

到这里，已经基本可以看到Hooks的优势了，那么，React发布Hooks的真正原因又是什么呢？

# Motivation
1. 很难在组件间重用处理状态逻辑代码
拿我们常用的聊天软件QQ来作比喻（QQ虽然不是使用React写的，但是可以使用组件化的思想来分析）：
首先在信息界面看到每一个聊天的右方都有一个最近聊天时间，我们暂时将它称为组件A，然后点进去了之后可以看到另外一个最近发言的时间，我们称它为组件B。由于这两个时间都是由接近相同的处理函数来进行逻辑处理的，在这里可以简单地理解为二者都是通过发送https请求服务器，然后获得时间。这两个组件因为处理的逻辑非常相似，但是因为不处于一个组件，所以没有很好的方法去重用。
过去，我们是通过使用`render props`（render属性）和`higher-order components`（高级组件），而Hooks能够帮我们重用这些逻辑代码而不改变组件的结构，构建更加简洁的组件。

2. class中复杂的组件难以理解
class组件中有很多生命周期方法，但是因为他们调用时间的问题，使得有时候处理同一个逻辑的代码分散在不同的生命周期函数里面，这就造成了组件难以理解。
Hoo

3. class困扰了很多人
至今学习React的开发者想必都有被React中function和class组件的区别，this的问题困扰过。

以上三个理由就是React发布Hooks的主要理由，详细内容可以上官方网站查看。

# Hooks的使用
## useState
那么讲了那么多，`Hooks`究竟怎么使用？Show me the code!
还是回到刚刚的例子：
```js
import { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
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

可以看到Hooks使用了一个新的API`useState`
> 还有另一个API`useEffect`

`useState`接受一个参数，用来定义一个初始值（这个初始值跟class中this.state类似，但有略微不同，这里可以直接传进任何类型，如数字、字符串而不仅局限于对象）
返回一个数组（里面有一对state以及处理state的方法，类似于class中的setState方法），所以上面使用了数组的解构语法。

另外，可以同时声明多个Hooks state：
```js
function ExampleWithManyStates() {
  // Declare multiple state variables!
  const [age, setAge] = useState(42);
  const [fruit, setFruit] = useState('banana');
  const [todos, setTodos] = useState([{ text: 'Learn Hooks' }]);
  // ...
}
```

## useEffect
> Effect在这里是side effect的意思，这个方法的作用是处理一些会产生副作用的代码，比如发起http请求，操作DOM对象等

`useEffect`接受一个参数函数，用来处理有副作用的代码，同时，如果这个函数返回了另一个函数，那么这个函数就可以在React在每次render之后’取消订阅‘这个函数（比如说取消ajax请求等），然后再执行一次新的有副作用的代码。
它的第二个参数就是传进一个，如果这个值在render之后与render之前没有发生改变，React就会跳过这次的副作用代码执行。

```js
 useEffect(() => {
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    // Specify how to clean up after this effect:
    return function cleanup() {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });
```
