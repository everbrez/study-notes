# CommonJS
## 1.模块引用
```js
const fs = require('fs');
```
## 模块定义
上下文提供了`module.exports`对象用于到处当前模块的方法或者变量，并且它是唯一的导出出口。
```js
const sayHello = name => `Hello! ${name}`;
module.exports.sayHello = sayHello; 
```
在另一个文件引用
```js
const greet = require('./greet');
greet.sayHello('Nike'); // Hello! Nike
```
## 模块标识
模块标识其实就是传递给`require()`方法的参数，它必须是符合驼峰命名的字符串（核心模块），或者由`./`、`../`开头的相对路径，或者绝对路径。同时它可以没有后缀`.js`

## 2.Node的模块实现
在Node中引入模块，需要经历如下三个步骤：
1. 路径分析
2. 文件定位
3. 编译执行

> 在Node中，模块分成两类：一类由Node提供，称为核心模块；另一类由用户编写，称为文件模块。
- 核心模块部分在Node源代码编译过程中，编译进了二进制执行文件。在Node进程启动时，部分核心模块就会被直接加载进内存中，所以这部分核心模块的引用时，文件定位和编译执行这两个步骤可以省略，并且在路径分析中优先判断，所以它的加载速度是最快的。

### 2.1优先从缓存加载
不论是核心模块还是文件模块，`require()`方法对相同模块的二次加载都一律采用缓存优先的方式。（核心模块的缓存检查先于文件模块的缓存检查）。

> Node缓存的是编译和执行之后的对象。

### 2.2路径分析和文件定位
#### 模块标识符分析
模块标识符在Node中主要分为以下几类：
- 核心模块（优先级仅次于缓存加载）
- 相对路径文件模块
- 绝对路径文件模块
- 非路径形式的文件模块，如自定义的connect模块

> Node在分析路径形式的文件模块时候，会将路径转为真实的路径，并以真实路径作为索引，将编译后的结果存放到缓存中。

> 自定义模块可能是一个文件或者一个包，这类模块的查找最费时，也是所有方式中最慢的一种。

执行命令：
```js
console.log(module.paths); //["e:\Personal\dyf\node-demo\node_modules", "e:\Personal\dyf\node_modules", "e:\Personal\node_modules", "e:\node_modules"]
```
可以看出模块路径生成规则顺序:沿路径向上逐级递归，直到根目录下的node_modules目录。

#### 文件定位
- 文件扩展名分析(按.js、.json、.node补足扩展名)
- 目录分析和包（如果分析扩展名之后，得到了一个目录，此时Node会将目录当成一个包来处理）
> 在这个过程中，首先Node在当前目录下查找`package.json`，通过`JSON.parse()`解析出对象，从中取出`main`属性进行定位。如果`main`属性指定的文件名错误，或者压根没有`package.json`文件，Node会将index当做默认文件名，然后依次查找index.js、index.node、index.json。

### 2.3模块编译
Node中，每个文件模块都是一个对象，定义如下：
```js
function Module(id, parent) {
  this.id = id;
  this.exports = {};
  this.parent = parent;
  if (parent && parent.children) {
    parent.children.push(this);
  }

  this.filename = null;
  this.loaded = false;
  this.children = [];
}
```
1. JavaScript模块的编译
在编译的过程中，Node对获取的JavaScript文件内容进行了头尾包装。添加了
```js
(function (exports, require, module, __filename, __dirname) {\n {your code}\n});
```
> `exports` 和 `module.exports` 的区别：`exports`是形式参数，改变其引用并不能传递出去，而改变`module.exports`的引用可以传递出去

2. JSON文件编译
Node利用fs模块同步读取JSON文件的内容之后，调用`JSON.parse()`方法得到对象，然后将它赋给模块对象的`exports`，以供外部调用。


## AMD 和 CMD
### AMD 
```js
define(id?,dependencies?,factory); // factory 就是实际代码内容
```
> AMD需要在声明模块的时候指定所有依赖，通过形参传递依赖到模块内容中：
```js
define(['dep1','dep2'],function(){
    return funtion(){}
});
```

### CMD
```js
define(factory);
```
> CMD支持动态引入

```js
define(function(require,exports,module){
    // some codes
});
```
