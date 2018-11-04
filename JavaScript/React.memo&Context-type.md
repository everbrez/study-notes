## React.memo
React.memo是一个高阶组件，它的作用对象是`function` component而不是`class`component。
> `class` component可以使用`React.PureComponent`来实现类似的效果

### 作用
如果`function`component对于同样的props输入，将`render`一样的结果，那么就可以使用`React.memo`来提升性能。
`React.memo`可以让React对于同样的props输入，跳过渲染组件，直接复用上一次渲染的结果。

### 用法
```js
function MyComponent(props) {
  // render...
}

export default React.memo(MyComponent)
```
一般地，`React.memo`只会对props对象进行浅比较(shallow),如果需要进行控制比较的过程，可以提供第二个参数来进行控制。
```js
React.memo(MyComponent, (prevProps, nextProps) => {
  //... return true or false to control the comparison
})
```

## static contextType
传统的Context API用法一般如下
```js
const MyContext = React.createContext()
```
现在React增加了一个新的更加方便的方式来使用Context
```js
class MyClass extends React.Component {
  static contextType = MyContext

  componentDidMount() {
    let value = this.context
    //...
  }
  //...
}
```
