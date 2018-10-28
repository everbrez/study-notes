## Background
以前，开发者使用生命周期函数`componentWillReceiveProps`来更新由`props`变化引起的状态，但是从React 16.3发布之后，引入了一个用来代替`componentWillReceivePorps`的另外一个生命周期函数：`getDerivedStateFromProps`。
> 在16.3版本被取代的生命周期函数有：`componentWillMount`，`componentWillReceiveProps`，和`componentWillUpdate`，同时增加了两个生命周期函数：`getDerivedStateFromProps`和`getSnapshotBeforeUpdate`。同时，被取代的生命周期函数直到 17 版本之前还可以正常工作，在 17 版本之后，虽然还可以使用它们，但是需要加上前缀`UNSAFE_`来使用它们。

更新之后的生命周期函数可以从官方提供的`react lifecycle methods diagram`查看[http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/]

## getDerivedStateFromProps
`getDerivedStateFromProps`的存在意义是在`props`更新的时候改变其内部状态`state`。但是`getDerivedStateFromProps`应该更加谨慎地使用，一般使用不当会出现下面的问题：
- 当`props`改变时无条件地更新`state`
- 当`props`与`state`不相同的时候总是更新`state`

**其实`getDerivedStateFromProps`函数没有想象中需要使用地那么频繁，但是开发者经常滥用（overuse）它，导致了出现很多异常的情况**

开发者在使用`getDerivedStateFromProps`时候会经常混淆两个数据--`props`和`state`
> 通过`props`进行传递的数据一般称为`controlled`，而处于组件内部`state`的数据一般称为`uncontrolled`，这是因为`props`对于组件来说是不可更改的，它完全受控制于父组件，而`state`对于组件来说是可以更改的（比如使用`setState`来更新`state`的数据）

那么当一个组件的`state`value同时受到组件内部的`setState`以及外部的`props`影响的时候，即当`state`的数据来源不只是完全受控制于组件内部`setState`，也部分受控制与外部`props`的时候，也许就是需要重构代码的时候。

### 无条件地将`props`复制到`state`
由生命周期函数的图[http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/]可以看出，`getDerivedStateFromProps`贯穿于创建和更新操作，这意味着，不仅仅是`props`的变化会引起这个函数的调用，当组件在挂载，使用`setState`等更新状态的时候都会引起它的调用。
> 这个是`getDerivedStateFromProps`与`componentWillReceivePorps`不一样的地方，`componentWillReceiveProps`只会在`props`发生变化的时候调用。另外，`componentWillReceivePorps`会在父组件每次更新的时候调用，着就会导致了无论`props`有无变化，都会执行这个函数。它被标记了`unsafe`，就是因为它的这种行为会导致`state`的丢失

下面看一个`state`丢失的例子（来自官网）
```js
class EmailInput extends Component {
  state = { email: this.props.email }

  render() {
    return (
      <input type="text" onChange={this.handleChange} value={this.state.email} />
    )
  }

  handleChange = event => {
    this.setState({
      email: event.target.value
    })
  }

  componentWillReceiveProps(nextProps) {
    this.setState({
      email: nextProps.email
    })
  }
} 
```
在这个例子中，如果用户输入了一个邮箱，然后此时父组件`render`被调用，因为`componentWillReceiveProps`无论`props`有无变化都会调用，所以`EmailInput`的`state`就会被重置，输入框的内容便会初始化。

下面是一个改进的例子
```js
class EmailInput extends Component {
  //...
  componentWillReceiveProps(nextProps) {
    if(nextProps.email !== this.email) {
      this.setState({
        email: nextProps.email
      })
    }
  }
}
```

### 更好地解决方法
1. 将组件变成`fully controlled`组件，这意味着组件的状态将完全受父组件的控制，没有内部的状态。
```js
const EmailInput = props => (
  <input onChange={props.onChange} value={props.email} />
)
```
> 将`fully controlled`组件用纯函数实现可以优化性能，避免从`React.component`上继承过多的属性和方法（如生命周期函数）等

2. 将组件变成`fully uncontrolled`组件，同时使用`key`来实现更方便的重置操作
```js
class EmailInput extends Component {
  state = { email: this.props.defaultEmail }

  handleChange = event => {
    this.setState({
      email: event.target.value
    })
  }

  render() {
    return <input type="text" onChange={this.handleChange} value={this.state.email} key={this.props.user.id} />
  }
}
```
> 当`key`发生改变的时候，React会创建一个新的组件实例而不是更新现有的实例。这意味着，当`this.props.user.id`发生改变的时候，这个实例的所有数据（包括state）都会被丢弃，然后重新创建一个新的实例，然后再次初始化。虽然看起来会对性能造成很大影响，不过实际上的影响却是极小的，相反，如果将数据重置（reset）的操作很繁琐的时候，使用`key`进行重置往往会获得更好的性能

3. 当`fully uncontrolled`不能使用`key`来重置数据，或者使用这种方法重置数据对性能影响很大的时候，就轮到`getDerivedStateFromProps`生命周期函数的登场了。
使用这个生命周期函数的关键是：明确当`props`中什么发生改变的时候，`state`应该随之改变。
下面是一个优化后的例子
```js
class EmailInput extends Component {
  state = {
    email: this.props.defaultValue,
    userID: this.props.user.id
  }

  getDerivedStateFromProps(props, state) {
    return props.user.id === state.userID
    ? null 
    : {
      email: props.defaultValue,
      userID: props.user.id
    }
  }
  //...
}
```
> 在React中，getDerivedStateFromProps被设计为纯函数，即没有`side effect`副作用。这是React为后面推出的异步渲染做铺垫

4. 当没有像上述的`props.user.id`来识别是否重置`state`的时候，通常可以让父组件通过React的API`Ref`来强制重置，这需要在子组件内部实现好重置状态的方法。
```js
class Email extends Component {
  state = {
    email: this.props.defaultEmail
  }

  resetEmail(newEmail) {
    this.setState({
      email: newEmail
    })
  }

  //..
}
```

## Conclusion
1. 判断`controlled`和`uncontrolled`数据，然后选用不同的方式创建组件，一般有以下两种简单的组件：
- fully controlled component
- fully uncontrolled component
当然也有部分`controlled`的组件

2. 不同组件有不同的`reset state`方法，可以根据实际情况选用
3. 对于`uncontrolled`，推荐使用`key`来重置所有的state
