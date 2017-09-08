# 3-1-Redux 简介

记得在课程开始的时候我们就讲到，我们使用React的主要情境不是内容网页，而是Web应用。Web应用与我们传统的新闻站、博客等类型的网站有很大的不同。Web应用会涉及到非常多的数据交互、异步传输、甚至我们需要让所有的用户交互和应用功能在同一个页面里完成，也就是开发单页面应用。

换句话讲，我们要开发的Web应用会涉及非常多的状态state改变。包括与服务器的数据交互，界面上导航或其他部件的切换、处理用户输入等等。可以想象，等到我们的应用复杂到一定程度时，如果应用的状态分散在各个组件当中，有的组件只控制自己样式改变的状态，有的组件之间需要相互通信，传递数据。各种各样的数据，状态改变错综复杂，后期维护起来简直会是一场噩梦。万一产品经理这时候再提出说要改几个需求，添加个功能，跳楼的心估计都有了。

之前我们介绍过了React当中有状态组件和无状态组件的概念，React本身也推荐我们尽量控制有状态组件的个数，开发过程中我们编写的组件90%都应该是无状态组件，然后通过props来接收数据。

React本身只留给了我们state和props来管理应用数据的状态，context只是实验性质的一种传递数据的方式。也就是说，React本身并没有很好地提供应用状态管理的解决方案，在一些小应用中我们可能感觉不到状态管理是一个问题，但是随着应用的复杂度增加，这个问题会暴露得越来越明显。这时候我们就需要使用到Redux

既然React推荐我们尽量少得编写有状态组件，那么我们何不干脆把一个应用的数据状态统一在一个地方管理？这便是Redux的理念。

Redux把应用的数据统一储存在一个全局对象中。把应用所有的数据交互和状态改变，统一用固定形式的action动作对象来描述。经由名为Reducer的方法来判断不同的动作如何改变应用状态。最后，我们通过Store对象来负责执行action动作，获取state应用状态、订阅状态改变时触发的事件。

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

class Counter extends Component {
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
        {this.state.value}
        <button onClick={this.increment}>+</button>
        <button onClick={this.decrement}>-</button>
      </div>
    )
  }
}
```

事实上，Redux提供的更多的是一种应用状态管理问题的解决思路。它的源码很少，只有几kb，我们甚至可以用一些非常基本的函数方法来模拟实现redux的全部功能。并且，即使我们在应用中引入Redux，编写的大部分也都是原生的JS函数和对象而已。也就是说，只要你能够掌握Redux的理念，你甚至无需使用Redux这个库本身，也能够通过它的方式来编写代码解决问题。换句话讲，你可以用自己的代码实现 Redux 的所有核心功能，所以 Redux 也是最适合拿来教学的一个状态管理库。

可能实践中 Redux 在处理数据的流程上有一些繁琐，在对一些比较复杂（例如带有异步）的操作方面比较吃力，但学习 Redux 仍然能让你对应用的状态管理这一方面的功能有一个全面的理解。

在正式开始学习Redux之前，还有一点需要提醒各位同学。任何一个新的框架和库的出现都是为了解决某些或某个特定的问题。我们使用一个框架或库也应该秉持“我是为了解决遇到的问题才使用这个框架或库”的原则。而不是因为某个框架比较流行比较火，在根本没有了解其出现的背景下就开始盲目使用。我们的教程只是为了帮助大家掌握React技术栈的使用方法和理念，而在实际的工作中是否要运用我们了解到的这些工具，还需要具体情况具体分析，等到我们真正遇到了实际的问题时，再拿出相应的库来解决。

要记得，永远不要为了用框架而用框架。本教程的目的也只是为了教给你React主要技术栈的使用方法，并不是告诉你，以后只要开发Web应用就必须使用我们所学的这些工具库，而是在你真正遇到需要这些工具库来解决的问题时再去实际应用。



