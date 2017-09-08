# React & Redux 第一讲 react新版本及ES6简介

1. 课程简介
2. Getting Started变化
    JSXTransformer
    Create-react-app
3. React语法变化
    Component
4. ES6基本语法
    import
    const
    Class
    Arrow Function

第一节课呢，我们就来简单介绍一下这些应用在React开发当中，属于 ES6 的新的关键字和语法糖，也好为我们之后的学习打下一个基础，当然我更推荐同学们对 ES6 有一个比较全面的了解之后再开始学习 React，不过你也不需要担心，以后在课程中每当遇到涉及 ES6 新语法的问题时，我都会稍作讲解。

**[ECMAScript 6 入门](http://es6.ruanyifeng.com/)**

## import

```jsx
import React, { Component } from 'react';
```

我们就从头开始看。首先是在JS当中引用其他库文件的语法变化。之前我们一般都是通过 `require` 方法把库文件导出的方法保存在一个变量中。在ES6当中引入了一组个新的关键字 `import/export`，如果有同学对 Java 或 Python 有所了解的话，对 `import` 语句应该不会感到陌生，一般我们都会在文件的开头引入我们需要使用的模块或方法。

我们在一个文件中导入的模块或方法是从另一个文件中导出的。如果是使用 `export default` 语句导出的方法，我们直接定义其变量名称，这样的方法每个文件只能导出一个：

```js
// myFunction.js
export default function() {
    console.log('This is default function!');
}
// index.js
import myFunction from 'myFunction';
```

仅用 `export` 导出的方法，在使用时，则需要把它们包含在大括号里：

```js
// myAnotherFunction.js
export const foo = 'bar';
export function bar() {
    console.log('foo');
}
// index.js
import { foo, bar } from 'myAnotherFunction';
```

## const

```jsx
const title = <h1>React Learning</h1>
```

`const` 关键字在ES6语法中，被用来声明常量。不过这并不表示声明的常量中数据不可变。在es6中，`const` 声明的其实是一个只读的指针，也即是指针的位置不能改变，但其指向的值事实上是可以操作的，我们来看一下下面这个例子：

```js
// 我们先来定义一个常量空数组 a 
const a = [];
// 输出空数组的内容
console.log(a);
// 返回结果 []
// 通过下面的操作可以向数组添加内容
a.push(2);
// 输出添加了元素之后的内容
console.log(a);
// 返回结果 [2]
// 但假如我想要将 a 指向一个新的数组则会报错
a = [];
// Uncaught TypeError: Assignment to constant variable.
```

上述代码你可以打开 Chrome 控制台自己输入测试体验一下。

我们用JSX创建的元素一般是不变的，所以通过 `const` 关键字来声明一个 React 的元素，而不是我们以往经常使用的 `var`

## class

在 React 之前的版本当中，我们一般使用一个名为 `createClass` 的方法来声明一个组件（现在这一API已经移除，如果非要使用的话需要单独引入名为 create-react-class 的包）。而在 ES6 当中，我们可以通过 class 关键字继承 React组件 的原型来创建组件。

在使用 props 或 state 的组件中，我们都会看到一个名为 constructor 的方法，这个方法中一般都会调用一个叫做 super 的关键字。这里的 super 其实是为了调用其继承父类的构造函数，由此将子类的 this 初始化，这样我们才能够在后面的代码中调用 this.props 或者 this.state

这么讲很多同学可能还是不太明白，不如我们直接来做一下实验：

```js
class Animal {
  constructor(name) {
    this.nickName = name
  }
}

class Cat extends Animal {
  constructor(name) {
    super(name)
    document.write(this.nickName)
  }
}

const myCat = new Cat('Tom')
// 这时我们就可以在子类 Cat 中访问父类 Animal 的 nickName 属性。你可以用在线编辑器注释掉 super 方法来测试它带来的影响。
```

[在 Codepen 上试试](https://codepen.io/discountry/pen/aJLvOe)

## 箭头函数

箭头函数arrow function 是 ES6 中很棒的一个新特性，可以让你少写很多代码。从此在代码中几乎不用再拼写 function 了（当然并不是说 arrow function 适用于所有场景）。在 React 当中使用箭头函数还有别的功效，我们来看一个简单的计数器的例子：

```jsx
class IncrementWidget extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      num: 0
    }
    // 我们可以通过 bind 方法手动绑定 this
    // this.handleClick = this.handleClick.bind(this)
  }
  handleClick() {
    this.setState({
      num: this.state.num + 1
    })
  }
  // 我们也可以直接在声明事件处理函数的时候使用箭头函数语法绑定 this
  /*
  handleClick = () => {
    this.setState({
      num: this.state.num + 1
    })
  }
  */
  render() {
    return (
      <div>
        <p>{this.state.num}</p>
        {/* 因为是匿名函数，所以会导致每次更新时组件都会重新渲染，这样写代码最简洁但是对性能有影响。 */}
        <button onClick={() => this.handleClick()}>Add</button>
      </div>
    )
  }
}

ReactDOM.render(<IncrementWidget />, document.getElementById('root'))
```

[在 Codepen 上试试](https://codepen.io/discountry/pen/VpGRpP)

假如不使用 bind 我们会发现这一段代码是无法正常工作的，但是采用之前的 createClass 方法则不会，这是因为在 ES5 语法中，React 已经默认为所有方法绑定了 this，而在用 class 声明的组件中则不会有这种默认绑定。

给所有的事件处理函数加上 `bind(this)` 确实很不爽。

这时候箭头函数就派上了用场，只需要按照上述示例当中的语法书写，this 也就会被自动绑定到函数中去啦。方法任选其一，至于你在实际的项目当中想要怎么写还是看你的个人习惯或者是项目需求了。

## 总结

最后让我们来复习一下本节课所学的内容。

首先，我们介绍了 import 和 export 语句，在实际的开发过程中，我们肯定不能把所有的代码都写在一个文件当中，在 ES6 中新加入的 import 和 export 可以方便我们很好地组织引用代码的各个模块。

然后我们介绍了 const 关键字，一种新的定义常量的方法，常量事实上定义的是一个只读的指针，假如我们指向的是一个数组或者对象，还可以通过一些方法例如 array.push 操作其中的数据。

接下来我们介绍了 JS 当中的 class 类的实现，通过 class 来声明一个 React组件 和之前通过 createClass 方法创建组件有很多区别，我们又对 constructor 方法和 super 关键字稍做了解释，super 主要是用来调用父类的构造函数，以使得我们在子类中能够正确地获取 this

最后我们介绍了 arrow function 箭头函数，这其实相当于是一种实现函数缩写的语法糖，可以很方便地让我们少写很多代码。另外在 react 组件中使用箭头函数来声明事件处理方法也有实现自动绑定 `this` 的功能。 