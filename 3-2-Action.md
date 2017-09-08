# 3-2-Action

* action
    - action type
    - action payload
* action Creator
* action Creator Generator

我们还是来看上节课计数器的例子：

```jsx
// Reducer
const counter = (state = { value: 0 }, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { value: state.value + 1 };
    case 'DECREMENT':
      return { value: state.value - 1 };
    default:
      return state;
  }
}

class Counter extends React.Component {
  // State
  constructor() {
    super();
    this.state = counter(undefined, {});
  }
  
  dispatch(action) {
    this.setState(prevState => counter(prevState, action));
  }
  // Actions
  increment = () => {
    this.dispatch({ type: 'INCREMENT' });
  };

  decrement = () => {
    this.dispatch({ type: 'DECREMENT' });
  };
  
  render() {
    return (
      <div>
        <p>{this.state.value}</p>
        <button onClick={this.increment}>+</button>
        <button onClick={this.decrement}>-</button>
      </div>
    )
  }
}

ReactDOM.render(<Counter />,document.getElementById('root'))
```

我们现在只看有关dispatch的这一部分：

```js
dispatch(action) {
    this.setState(prevState => counter(prevState, action));
  }
  // Actions
  increment = () => {
    this.dispatch({ type: 'INCREMENT' });
  };

  decrement = () => {
    this.dispatch({ type: 'DECREMENT' });
  };
```

拿其中的increment方法来说，我们相当于将一个对象传递到了counter函数当中：

```js
this.setState(prevState => counter(prevState, { type: 'INCREMENT' }));
```

这里的带有type属性的js对象`{ type: 'INCREMENT' }`就是我们在Redux中定义的action动作。在形式上，action就是带有type属性的JS对象。在Redux的约定中，我们要将所有改变应用状态的操作规范为一个个action，在action中，我们也可以附上要用来修改应用状态state的数据：

```js
{ type: 'ADD_TODO', text: 'Use Redux' }
{ type: 'REMOVE_TODO', id: 42 }
{ type: 'LOAD_ARTICLE', response: { ... } }
```

例如上述的这个例子也可以改写为下面这样，我们将自己操作数据的部分去掉，之后根据action传递的具体值来增减状态数据：

```jsx
const counter = (state = { value: 0 }, action) => {
  switch (action.type) {
    case 'INCREMENT':
    case 'DECREMENT':
      return { value: state.value + action.num };
    default:
      return state;
  }
}

class Counter extends React.Component {
  ...
  
  dispatch(action) {
    this.setState(prevState => counter(prevState, action));
  }
  // Actions
  increment = () => {
    this.dispatch({ type: 'INCREMENT', num: 1 });
  };

  decrement = () => {
    this.dispatch({ type: 'DECREMENT', num: -1 });
  };
  
  ...
}
```

根据代码，我们了解到，目前为止，我们的action有两个作用，一个是定义我们的应用可以进行的动作或操作的类型，另一个是传递改变应用状态的数据。在Redux的约定中，action只有type属性是必须包含的，其他的数据如何定义全在于你想要如何使用，当然如果你希望你定义的action能够规范一些的话，也可以遵从[Flux Standard Action](https://github.com/acdlite/flux-standard-action)的标准：

```js
{
  // action 类型
  type: 'INCREMENT',
  // payload 中返回我们要传递的数据，用来修改应用 state
  payload: {
    num: 1  
  },
  // payload 数据未获取成功时返回 true
  error: false,
  // 一些不必要在 payload 中传递的其他数据
  meta: {
    success: true
  }
}
```

根据Flux标准，action必须是JS对象，必须包含type属性，可以有payload/error/meta几个属性，除此之外不允许有任何其他属性。在开发应用的过程中，实施规范可以更好地方便团队之间的协同，而不至于不同的开发者写出各种奇奇怪怪的action来。

再进一步，为了防止我们的代码中出现很多写死的数值，减少hard code，可以使用在Redux中称之为action creator的方法来创建action

```js
function addNumber(num) {
    return { type: 'INCREMENT', num }
}

function minusNumber(num) {
    return { type: 'DECREMENT', num }
}

...

increment = () => {
    this.dispatch(addNumber(1));
  };

  decrement = () => {
    this.dispatch(minusNumber(-1));
  };
```

继续将我们的代码抽象，我们可以提炼出生成action creator的generator

```js
/*function counterActionGenerator(type, num) {
  return function(num) {
    let action = { type, num : num };
    return action;
  }
}*/

const counterActionGenerator = (type, num) => (num) => {
    let action = { type, num : num }
    return action
}

const addNumber = counterActionGenerator('INCREMENT', null)
const minusNumber = counterActionGenerator('DECREMENT', null)
```

另外，为了更清晰地表达我们应用可以进行的所有action动作，还可以在应用的开头定义所有的action type

```js
// actionTypes.js
export const INCREMENT = 'INCREMENT'
export const DECREMENT = 'DECREMENT'

// action.js
import { INCREMENT, DECREMENT } from './actionTypes'

const counterActionGenerator = (type, num) => (num) => {
    let action = { type, num : num }
    return action
}

const addNumber = counterActionGenerator(INCREMENT, null)
const minusNumber = counterActionGenerator(DECREMENT, null)
```

普通人第一眼看到这样的定义时肯定会觉得写这样代码的人脑子有毛病，把字符串定义成同名的常量没有任何意义。在小型项目中，定义这些类型的常量可能确实有些多费手续，但是在一些需要团队合作的大中型项目中，预先定义好应用的action类型有许多好处：

* 一个是我们可以在同一个文件中规范应用所有action的命名（因为你的程序不是说写出来就完了，更多的时候还需要后期维护更新）
* 定义action类型的文件类似于一个说明文档，当你想要为应用添加新特性时，可以查阅已有的应用action类型，这样可以避免冲突（我们在正式的开发当中肯定需要相互协作，添加功能的时候也不可能随性添加，要遵从之前的标准和规范）
* 每次代码提交到版本库时，我们也可以在这个文件中很轻松地查阅那些action类型被增加修改或删除了
* （可以减少一些typo错误）假如你打错字的话，在import这一步你的代码就会报错，这样方便你更快地调试，而不是在程序运行后才发现一些字符串错误。

不过话说回来，你的代码具体怎么组织，还是看你开发项目的实际需求，没有必要强行应用某种约定，一切都以解决实际问题为出发点。


