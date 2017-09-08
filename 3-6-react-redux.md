# 3-6-react-redux

这节课中，我们将学习React与Redux的协同使用，在前面几节课的示例当中，我们要么是只使用了react或者单独使用了redux，接下来呢，让我们来继续重构之前的计数器的例子，不过这一回呢，我们要正式地协同使用react和redux了：

```jsx
const counter = (state = 0, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1;
    case 'DECREMENT':
      return state - 1;
    default:
      return state;
  }
}

const { createStore } = Redux;

const store = createStore(counter);
console.log(store.getState());

/* const render = () => {
  document.body.innerText = store.getState();
}; */

const Counter = ({
    value, 
    onIncrement,
    onDecrement
    }) => (
    <div>
        <p>{value}</p>
        <button onClick={onIncrement}>+</button>
        <button onClick={onDecrement}>-</button>
    </div>
    )

const render = () => {
    ReactDOM.render(
        <Counter
      value={store.getState()}
      onIncrement={() =>
        store.dispatch({
          type: 'INCREMENT'           
        })            
      }
      onDecrement={() =>
        store.dispatch({
          type: 'DECREMENT'           
        })            
      }
    />,
        document.getElementById('root')
        )
}

store.subscribe(() => { console.log(store.getState()) })
let unsubscribe = store.subscribe(render);

render();
```

redux部分的代码呢，几乎不需要怎么改动，我们还是使用一样的reducer创建一样的store.react是专门用来构建用户界面的框架，那么自然而然，我们在这里需要改动的，是我们之前直接用原生js实现的render方法。另外，希望同学们还没有忘了，使用react进行开发时的最佳实践，用展示组件和容器组件将我们界面的展示结构和数据处理分开来进行。我们先来写比较简单的展示组件，在这里，我们只需要3个界面元素，一个用来显示数字，再加上两个按钮，一个用来增加一个用来减少。

展示组件当中的所有数据和事件处理函数都是通过props传递过来的。在前面几节课当中，我们一直都在使用ES6的新特性，解构赋值。在react组件中我们同样可以使用，在传入参数中直接使用解构赋值的方法获取props，比方说我们这里，获取到的直接就是对应的props.value/props.increment等。

好，写到这里呢。我们先来观察一下我们需要用props传入的3个参数，在之前我们单独使用react的时候，我们是通过react自身的state来控制状态数据的，而现在有的redux，我们可以发现，其实我们需要的value就是redux目前返回的state，另外两个事件函数increment和decrement其实也就对应着同种类型action的执行。也就是说，目前的这3个参数，我们都可以直接从redux当中获取到。

那么接下来我们就直接试着改写之前的render函数，在这里调用ReactDOM的render方法，给value传入store.getState()也就是状态数据，而onIncrement方法就相当于是dispatch了type为increment的action，与之相似，onDecrement方法也是类似。

好了，改写到这一步呢我们已经实现了用react对render方法的重构，我们来点击这两个按钮测试一下交互，我们的react和redux协同使用的应用以及可以正常运行了。

当然，如果这么写的话，每次state改变，我们都需要调用ReactDOM.render方法，这样的写法显然有些耗费效率，我们还可以继续优化我们的代码：

```jsx
const counter = (state = 0, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1;
    case 'DECREMENT':
      return state - 1;
    default:
      return state;
  }
}

const { createStore } = Redux;

const store = createStore(counter);
console.log(store.getState());

const Counter = ({
    value, 
    onIncrement,
    onDecrement
    }) => (
    <div>
        <p>{value}</p>
        <button onClick={onIncrement}>+</button>
        <button onClick={onDecrement}>-</button>
    </div>
    )

class CounterContainer extends React.Component {
  componentDidMount() {
    this.unsubscribe = store.subscribe(() =>
      this.forceUpdate()
    );
  }

  componentWillUnmount() {
    this.unsubscribe();
  }
  
  render() {  
    return (
      <Counter
        value={store.getState()}
        onIncrement={() =>
          store.dispatch({
            type: 'INCREMENT'           
          })            
        }
        onDecrement={() =>
          store.dispatch({
            type: 'DECREMENT'           
          })            
        }
      />
    )
  }
}

store.subscribe(() => { console.log(store.getState()) })

ReactDOM.render(
        <CounterContainer />,
        document.getElementById('root')
        )
```

这一步呢，我们要完整写出counter的容器组件。在CounterContainer容器组件当中，我们先把之前的render方法直接复制过来，这一部分是不需要改变的。但是这一回呢，我们就不能频繁地调用ReactDOM的render方法了。

之前单独使用react时，我们通过setState方法可以触发React的更新渲染，而目前呢，状态数据全部交由redux管理了，redux当中的状态数据发生改变时，响应的是我们通过subscribe方法绑定的listener，所以在这里我们需要绑定上一个合适的监听函数。

React专门为我们提供了主动触发更新渲染的方法forceUpdate，在这里呢，我们也要配合react的生命周期函数，在componentDidMount方法当中，通过store.subscribe绑定this.forceUpdate()方法，作为状态数据改变时的响应。之后为了让我们的代码更加完善呢，也可以在componentWillUnmount生命周期函数当中进行解绑。

这样改写完之后，我们就只需要调用一次ReactDOM的render方法，之后都通过更新渲染的操作来响应状态数据的改变了。

在这里呢，我们要明确一下，react是用来构建用户界面的框架，而redux则是为了解决应用的状态管理问题而专门开发出来的库。这两者之间没有必然的联系，不是说我使用react的时候就必须使用redux，也不是说使用redux就只能用react构建界面。只是说这两个框架和库在一起搭配使用非常的合适，所以它们才算在了同一个技术栈或生态圈里，而且react官方也专门为我们提供了连接react和redux的库，名字就叫作react-redux.

事实上，在正式的开发当中，我们可以使用React官方为我们提供的库react-redux来绑定React和Redux，通过其提供的Provider和connect方法，我们可以很轻松地传递Redux管理的state到React组件中，像刚才我们手写的countercontainer这种容器组件，也可以直接通过connect这些方法自动生成：

```jsx
const counter = (state = 0, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1;
    case 'DECREMENT':
      return state - 1;
    default:
      return state;
  }
}

const { createStore } = Redux;

const store = createStore(counter);
console.log(store.getState());

const { Provider, connect } = ReactRedux;

const Counter = ({
    count, 
    onIncrement,
    onDecrement
    }) => (
    <div>
        <p>{count}</p>
        <button onClick={onIncrement}>+</button>
        <button onClick={onDecrement}>-</button>
    </div>
    )
// 在开发模式下，如果不指定类型会报错
Counter.propTypes = {
  number: React.PropTypes.number.isRequired,
  onIncrement: React.PropTypes.func.isRequired,
  onDecrement: React.PropTypes.func.isRequired
}

const mapStateToProps = (state) => ({
    count: state
  })

const mapDispatchToProps = (dispatch) => ({
  onIncrement: () =>
          dispatch({
            type: 'INCREMENT'           
          }),
  onDecrement: () =>
          dispatch({
            type: 'DECREMENT'           
          })
})

const CounterContainer = connect(mapStateToProps, mapDispatchToProps)(Counter)

store.subscribe(() => { console.log(store.getState()) })

ReactDOM.render(
        <Provider store={store}>
          <CounterContainer />
        </Provider>,
        document.getElementById('root')
        )
```

**Provider**

Provider的作用就是向我们的应用传递store的一个容器组件，一般我们都会把它套在应用组件的最外层：

```jsx
ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

**connect()**

connect方法可以根据我们现有的展示组件自动生成一个容器组件。我们向其传递了两个方法作为参数：

* mapStateToProps(state)
    - 用来对应state和props，并且传入此参数时在state改变时会触发组件更新。
* mapDispatchToProps(dispatch)
    - 根据store的dispatch来传递相应的方法触发action事件。

其实就相当于connect在获取了Redux的store之后，再根据我们传入的方法，把我们需要的部分对应到props属性中，再传递到我们的组件当中。这样在组件里，我们就可以直接通过组件的props来访问Redux的store中的值和方法。

react-redux内部实现了很多优化，可以不用像我们之前手动调用render或forceUpdate方法频繁地重新渲染组件，而且这些优化是我们自己手写很难实现的，所以在正式的项目中，我们最好还是使用官方的这个库来进行react和redux的连接。


