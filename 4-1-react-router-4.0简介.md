# 4-1-react-router-4.0简介

在Web应用开发当中，前端扮演着越来越重要的角色，很多原本在后端负责的工作现在都可以放到前端来进行。例如对路由的控制。

在Web应用中，我们引入了一个新的概念，叫做前端路由。无需后端返回页面，我们就可以通过前端路由实现url的改变以及页面的切换。

较早前的实现前端路由的方法呢，比如说在angular1当中是通过hash的形式来处理的（也就是带#号的锚链接），前端路由实现的方法多种多样，比方说现在我们也可以通过操纵[DOM中的history对象](https://developer.mozilla.org/zh-CN/docs/DOM/Manipulating_the_browser_history)来实现。

为了更好地理解react-router，我们先来试着自己实现一下前端路由的功能。

其实我们简单拆解一下，前端路由主要需要实现两个基本的功能，一个是改变当前的url，另一个是根据改变了的url更改显示相应的页面内容。我们可以通过history.pushState方法来操作url地址。

至于根据改变后的url触发相关页面变化，浏览器本身会自带一个onpopstate事件，但是只有在我们点击返回或前进按钮时才会正常触发，所以我们需要自己动手实现一个绑定事件的功能。

在这里第一步呢，我们需要改写原本浏览器当中的history对象，在pushState方法被调用时，除了改变url呢，我们还需要让它触发onpushstate事件。我们这里呢，之间采用执行匿名函数的方法，完成对history对象的pushState方法的修改。

第二步呢，就需要来定义我们的window.onpopstate方法了，onpopstate这个方法呢，本来就是需要我们指向一个具体的函数的，在这里呢，我们只进行最简单的操作，在路由切换改变的时候呢，在页面当中显示出具体的路由。

接下来还有一步，就是为页面上的所有链接添加上事件绑定函数，这样在页面中的链接被点击之后才能够触发我们的pushState方法而不是直接从浏览器跳转了。

```js
// patch history pushState
(function(history){
    var pushState = history.pushState;
    history.pushState = function(state) {
        if (typeof history.onpushstate == "function") {
            history.onpushstate({state: state});
        }
        return pushState.apply(history, arguments);
    }
})(window.history);
// Add trigger function
window.onpopstate = history.onpushstate = function(event) {
  document.getElementById('state').innerHTML = "location: " + document.location + ", state: " + JSON.stringify(event.state);
}
// Bind events to all links
var elements = document.getElementsByTagName('a');
for(var i = 0, len = elements.length; i < len; i++) {
    elements[i].onclick = function (event) {
        event.preventDefault();
        var route = event.target.getAttribute('href');
        history.pushState({page: route}, route, route);
        console.log('current state', history.state)
    }
}
```

这样，每当history.pushState运行后，相应的绑定事件也会被触发，我们就实现了一个基本的前端路由功能。

react-router为我们提供了一些预置的可以实现前端路由功能的组件。使用方法与React的组件保持一致，只需要在我们的应用中添加一些JSX标签，就可以直接为我们的应用添加上前端路由功能。

react-router这个库比较特别，到现在为止已经发布了4个主要版本，而且不同的版本之间差异比较大，尤其是这次发布的4.0版本完全采用了新的理念来设计，这次最新发布的react-router4.0一共包含了3个主要的库：

* react-router-dom (for web)
* react-router-native (for RN)
* react-router (core)

react-router-dom和react-router-native相当于在之前的react-router之上又抽象封装了一层，需要我们关注来手动配置的部分更少了，如果是开发Web应用的话，在绝大多数情况下，我们只需要引入react-router-dom就可以了，并且不需要像之前一样手动调整history等配置。

引入相应的组件，然后添加在我们开发的应用中合适的位置，一切就大功告成啦。

这节课我们先来体验一下react-router的功能，随后的课程当中我们会详细地介绍每个组件的使用方法的。

我们从react-router-dom当中引入3个组件BrowserRouter, Route, Link，这3个组件都是react组件，react-router设计的非常好，它不是通过配置啊绑定啊这种语法来使用，而是直接可以像使用其他react组件一样，我们为其传入props参数来使用。

我们还是直接来看代码，首先我们把BrowserRouter放到最外层，这是给我们的应用添加路由功能的容器组件。之后我们可以直接在应用里面添加Link组件，Link组件的to属性就是对应到相应的路由地址的。然后就是使用route来添加正式的路由了，每一个路由的path属性也是对应着路由地址，这个和上面的Link如果相同的话，点击对应的Link就可以跳转的对应的路由当中，每个路由还对应着一个component属性，这就是当地址跳转到对应的路由时会被渲染出来显示的组件了。

除此之外呢，我们来看这里的Topic组件，首先肯定的是，路由是可以嵌套使用的，与此同时呢，我们还可以通过例如这里使用的match属性来获取一些和路由相关的信息。

总而言之，react-router的使用是非常方便的，和我们使用其他封装好的react组件几乎没有任何区别，下一节课呢，我们将会正式的介绍react-router当中几个比较重要的组件的使用方法。

```jsx
const { BrowserRouter, Route, Link } = ReactRouterDOM

const BasicExample = () => (
  <BrowserRouter>
    <div>
      <ul>
        <li><Link to="/">Home</Link></li>
        <li><Link to="/about">About</Link></li>
        <li><Link to="/topics">Topics</Link></li>
      </ul>

      <hr/>

      <Route exact path="/" component={Home}/>
      <Route path="/about" component={About}/>
      <Route path="/topics" component={Topics}/>
    </div>
  </BrowserRouter>
)

const Home = () => (
  <div>
    <h2>Home</h2>
  </div>
)

const About = () => (
  <div>
    <h2>About</h2>
  </div>
)

const Topics = ({ match }) => (
  <div>
    <h2>Topics</h2>
    <ul>
      <li>
        <Link to={`${match.url}/rendering`}>
          Rendering with React
        </Link>
      </li>
      <li>
        <Link to={`${match.url}/components`}>
          Components
        </Link>
      </li>
      <li>
        <Link to={`${match.url}/props-v-state`}>
          Props v. State
        </Link>
      </li>
    </ul>

    <Route path={`${match.url}/:topicId`} component={Topic}/>
    <Route exact path={match.url} render={() => (
      <h3>Please select a topic.</h3>
    )}/>
  </div>
)

const Topic = ({ match }) => (
  <div>
    <h3>{match.params.topicId}</h3>
  </div>
)

ReactDOM.render(<BasicExample />, document.getElementById('root'))
```




