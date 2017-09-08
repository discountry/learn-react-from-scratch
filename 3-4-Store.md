# 3-4-Store

这节课我们来介绍redux当中最核心的部分Store.

* Store
  - getState
  - dispatch
  - subscribe
  - unsubscribe
* createStore
  - reducer
  - defaultState

Store是我们储存状态数据state的地方。我们通过redux当中的createStore方法来创建一个store

```js
import { createStore } from 'redux'
import counter from './reducers'
const store = createStore(counter)
```

我们先从redux库当中引入createStore方法，之后使用const关键字定义store，我们需要给createStore方法中传入我们之前定义好的reducer函数，这样就可以创建出来一个存储reducer方法处理的状态数据的store

那么createStore到底做了一些什么操作呢？我们可以先把生成的store对象输出来看一下，我们会发现它提供了3个主要的方法，另外一个replaceReducer现在还不需要看，我们基本上用不到它。

其中getState方法是用来获取当前存储中的状态数据。

dispatch则是具体将传入的action操作执行并触发listeners事件监听器的地方。

subscribe则是用来添加listener的方法，同时它会返回一个unsubscribe的方法用来取消listener。这里的listener说白了其实就是当状态数据改变时会自动触发的一些事件。

前面我们就介绍过了，redux是一个非常简单的库，在这里我们可以模拟一下createStore的源码，来加深同学们对store的理解：

```js
const createStore = (reducer) => {
  let state
  let listeners = []
  // 用来返回当前的state
  const getState = () => state
  // 根据action调用reducer返回新的state并触发listener
  const dispatch = (action) => {
      state = reducer(state, action)
      listeners.forEach(listener => listener())
    }
  /* 这里的subscribe有两个功能
   * 调用 subscribe(listener) 会使用listeners.push(listener)注册一个listener
   * 而调用 subscribe 的返回函数则会注销掉listener
   */
  const subscribe = (listener) => {
      listeners.push(listener)
      return () => {
        listeners = listeners.filter(l => l !== listener)
      }
    } 

  return { getState, dispatch, subscribe }
}
```

我们可以看到，createStore接受reducer作为参数，返回的是一个包含上述的三个方法的对象。其中getState就非常简单了，直接返回我们的状态数据；而dispatch则需要进行两个操作，一个是根据传入的action具体调用reducer来获取新的状态数据，之后还需要触发所有的listener，响应状态数据改变之后的动作；最后是subscribe，则是用来添加注册listener的，同时它还会返回一个去除注销listener的方法。虽然我们这里是在模拟实现redux，这也几乎就是我们要使用的redux当中createStore方法的全部功能了。

好，之前的课程中呢，我们都是在模拟redux，应用redux的理念写了一些原生的js方法，这节课呢，我们来看一下redux这个库本身的基本使用方法，下面我们来一起编写一个Redux使用的基本例子：

```js
const counter = (state = 0, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1
    case 'DECREMENT':
      return state - 1
    default:
      return state
  }
}
// 这里使用的是ES6的解构赋值的方法
const { createStore } = Redux
// import { createStore } from 'redux'

const store = createStore(counter)
console.log(store.getState())

/* document.addEventListener('click', () => {
  store.dispatch({ type: 'INCREMENT' })
}) */

const render = () => {
  document.body.innerText = store.getState()
}

store.subscribe(() => { console.log(store.getState()) })
let unsubscribe = store.subscribe(render)

render()

store.dispatch({ type: 'INCREMENT' })
store.dispatch({ type: 'INCREMENT' })
store.dispatch({ type: 'DECREMENT' })

unsubscribe()
console.log('unsubscribed!')
```

我们还是实现一个最基本的计数器的功能，首先定义我们的reducer方法，我们使用const关键字声明一个名为counter的函数，传入的参数为state和action，注意到这里我们传入的state后面跟着一个赋值等于0，这里使用到的是ES6的新特性，默认参数，也就是说我们随后调用counter方法如果不传入state的话，它默认会传入0作为state的值。

之后我们写一个switch的逻辑结构，这里也相当于定了好了action的类型，如果是increment增加的话，我们就为state加上1，如果是decrement减少的话，就给state减去1，而如果传入的是我们应用没设计的其他动作，我们就不对state进行操作。

接下来我们从redux当中引入createStore方法，如果你和我一样是在浏览器中编写redux的代码的话，我们需要像之前引入react时一样，在js窗格的设置中引入redux库文件的链接，之后在我们的代码里用解构赋值的方法获取到redux库当中的createStore，这种赋值方法也是es6当中的新特性，可以让我们直接获取到目标对象当中的同名方法。

之后呢，就是调用createStore方法来定义好我们的sotre对象。

然后，我们编写一个render方法，好让我们的状态数据能够在页面当中显示出来。

我们使用store的subscribe方法，将render绑定到listener里面，这样每当状态数据改变时，render方法就会被自动触发，页面当中的显示就能随之更新。

```js
const counter = (state = 0, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1
    case 'DECREMENT':
      return state - 1
    default:
      return state
  }
}
// 这里使用的是ES6的解构赋值的方法
const { createStore } = Redux
// import { createStore } from 'redux'

const store = createStore(counter)
console.log(store.getState())

/* document.addEventListener('click', () => {
  store.dispatch({ type: 'INCREMENT' })
}) */

const render = () => {
  document.getElementById('root').innerText = store.getState()
}

store.subscribe(() => { console.log(store.getState()) })
let unsubscribe = store.subscribe(render)

render()

document.getElementById('increment').addEventListener('click',function() {
  store.dispatch({ type: 'INCREMENT' })
})

document.getElementById('decrement').addEventListener('click',function() {
  store.dispatch({ type: 'DECREMENT' })
})

document.getElementById('unsubscribe').addEventListener('click',function() {
  unsubscribe()
  console.log('unsubscribed!')
})
```

接下来，再为我们的示例添加一些可以交互的功能来改变状态数据，我们在这里添加了三个按钮，分别绑定increment方法、decrement方法以及unsubscribe方法。接下来我们来编写这三个方法。increment直接调用store的dispatch方法，并传入action.type类型为increment作为参数，decrement方法也是类似，传入decrement类型的action作为参数。而unsubscribe方法呢，我们在本节课开始的时候介绍了，store的subscribe方法返回的就是一个现成的unsubscribe方法，因此我们之间赋值刚才的绑定render的方法给unsubscribe就能够获取到它返回的函数。

接下来我们再点击这几个按钮实际操作一下。当我们的事件绑定函数被触发时，页面当中显示的状态数据也就自动更新了。

我们使用redux编写应用时，首要的就是定义一个具体的store对象，因为不管是action还是reducer都是通过store当中的方法具体被调用了，而获取store需要使用到createstore方法，这个方法需要传入reducer作为参数，所以我们在编写代码时需要先写出reducer方法。话又绕了回来，reducer方法需要判断action的类型，所以事实上，最先要被定义好的应该是各种类型的action，action又是用来传入修改state状态数据的参数的，所以最早定义好的应该是应用的state状态数据。

我们在开发稍具规模的一些应用时，肯定是要先规划好应用要处理的数据结构。使用redux时最好先在一个单独的文件中定义好基本的action类型，其实这也就相当于在设计我们的应用都有哪些功能，用户可以触发哪些功能，对数据会造成什么影响。接下来就是获取到store，给我们的用户界面上面添加响应用户操作的事件，以及事件触发之后状态数据改变，我们的界面又要如何跟着响应改变。这其实也就是使用redux进行开发的一般思路啦。