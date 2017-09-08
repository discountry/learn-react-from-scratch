# 3-5-Middleware

Middleware可以翻译为中间件，如果同学们对后端稍微有一些了解的话，对这个概念应该不会陌生，例如我们在查看特定的带有用户信息的网页之前需要登录验证，这个处理登陆验证的模块就可以被称为middleware中间件。

在我们开发Redux的过程中，也有类似的适用场景，比方说，每个action在触发之后，我们希望在控制台看到到底是哪个action被触发以及应用状态前后的变化：

```js
console.log('prev state', store.getState())
store.dispatch(action)
console.log('next state', store.getState())
```

最直接的办法当然是在调用dispatch之前输出一次，调用完再输出一次。这样写当然看起来比较傻，而且有一些hard code的性质。那么我们如何做才能让我们的控制台记录功能更优雅一些呢？其实我们可以手动修改创建store的dispatch方法：

在这里呢，我们先用next变量来暂存原本的dispatch方法，之后将dispatch赋值到新的方法，在这个新的名为dispatchAndLog的方法里呢，我们调用了两次控制台记录，在这中间呢，我们来调用next也就是之前原本的dispatch方法来实现我们的需求。

```js
const store = createStore(counter)
// 将原本的dispatch方法保留并附加上控制台输出的语句
let next = store.dispatch
store.dispatch = function dispatchAndLog(action) {
  console.log('prev state', store.getState())
  let result = next(action)
  console.log('next state', store.getState())
  return result
}
```

当然这种直接修改dispatch的方式也是不够优雅的，而且直接修改掉原本redux的原生方法其实也算是一种比较脏的处理。另外在现实中，我们肯定会面临一些同时使用多个middleware的情况，所以我们需要把middleware和dispatch的逻辑拆分开来：

我们试着来定义两个middleware，注意到这里middleware函数的写法，我们定义的方法返回了一个返回函数的函数。像我们上面的例子当中，我们想要给一个store加上middleware，必须先获取这个store，之后保存这个store原本的dispatch，在进行一些别的操作之后调用action作为参数传入dispatch，也就是说我们对store、dispatch、action的调用是一种固定的模式，那么我们就可以把这三项提炼出来都当作函数的参数依次传入使用，这也叫作函数的柯里化。

之后我们还需要定义一个名为applyMiddleware的方法，用来给我们的目标store加上特定的middleware，applyMiddleware方法接受两个参数，第一个参数是store，第二个参数则是middleware方法组成的数组。注意到这里我们需要先使用reverse方法颠倒middlewares数组的次序，这样才能保证它与action传递的次序一致。之后呢就是依次为dispatch加上middleware并返回了。

```js
let store = createStore(counter);

const confirmationMiddleware = store => next => action => {
    if (confirm('Are you sure?')) {
      next(action)
    }
    return false
}

const loggerMiddleware = store => next => action => {
    console.log('prev state', store.getState())
    let result = next(action)
    console.log('next state', store.getState())
    return result
}

function applyMiddleware(store, middlewares) {
  // 让middlewares的顺序调整成action传递的顺序
  middlewares = middlewares.slice()
  middlewares.reverse()

  let dispatch = store.dispatch
  middlewares.forEach(middleware =>
    dispatch = middleware(store)(dispatch)
  )

  return { ...store, dispatch }
}

store = applyMiddleware(store, [ loggerMiddleware, confirmationMiddleware ])
```

其实Redux本身已经为我们实现了非常完备的middleware支持，前面的这些代码演示只是为了让同学们更好地理解middleware的概念和其实现的原理，在实际的开发中，我们可以直接从redux当中引入applyMiddleware方法，使用的方式也是非常简单，在createStore方法中当作reducer之后的参数传入即可：

```js
const { createStore, applyMiddleware } = Redux

const store = createStore(counter, applyMiddleware(loggerMiddleware, confirmationMiddleware))
```
