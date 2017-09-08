# 4-2-react-router-4.0 配置

* BrowserRouter/Route
* Link/NavLink
* match

## Router/Route

在Web应用中，我们可以使用react-router-dom提供给我们的BrowserRouter组件，这个组件的功效和我们在上节课中演示的前端路由实现相同，主要就是实现页面内容与url同步的功能。

使用的方法也很简单，只需要把我们的应用组件嵌套在BrowserRouter组件当中即可：

```jsx
import { BrowserRouter as Router } from 'react-router-dom'

<Router>
    <App />
</Router>
```

在之前版本的react-router中，只为我们提供了Router组件，这一次全新的BrowserRouter组件已经内置了对浏览器的history支持，几乎不需要我们做任何配置就可以正常使用，而且使用的直接就是很漂亮的浏览器地址，没有＃也不会生成随机字符串之类的。

Route也就是我们的路由组件了，一般接受两个参数，path以及component来指定路由对应的url以及要渲染的组件：

同时也有我们经常会需要配置的一个属性，exact，比方说在下面这个例子当中，如果我们不声明exact属性，第一个路由只要是后面带有斜杠的地址全部都会匹配到，而加上了exact呢，它就只会匹配到根地址了。

```jsx
import {
    BrowserRouter as Router,
    Route
} from 'react-router-dom'

<Router>
  <div>
    <Route exact path="/" component={Home}/>
    <Route path="/blog" component={Blog}/>
  </div>
</Router>
```

## Link/NavLink

有了路由，我们的应用必然也需要导航，这样我们才能在应用的多个页面之间进行切换。react-router当然也很贴心地为我们提供了现成的导航组件。

Link是最基本的导航组件，一般我们只需要提供一个to作为参数指向其连接的路由：

```jsx
import {
    BrowserRouter as Router,
    Route,
    Link
} from 'react-router-dom'

<Router>
  <div>
    <ul>
        <li><Link to="/">Home</Link></li>
        <li><Link to="/blog">Blog</Link></li>
    </ul>

    <Route exact path="/" component={Home}/>
    <Route path="/blog" component={Blog}/>
  </div>
</Router>
```

但是通常我们在应用中，会为我们当前激活的导航设置不同的样式，这时就需要使用到NavLink，事实上NavLink也是在Link组件的基础上实现的，此外还附加了导航激活的相关属性：

比方说在这里，我们为NavLink加上activeStyle呢，在切换到对应路由的时候它就会加粗显示了。

```jsx
import {
    BrowserRouter as Router,
    Route,
    NavLink as Link
} from 'react-router-dom'

<Router>
  <div>
    <ul>
        <li><Link activeStyle={{ fontWeight: 'bold' }} to="/">Home</Link></li>
        <li><Link activeStyle={{ fontWeight: 'bold' }} to="/blog">Blog</Link></li>
    </ul>

    <Route exact path="/" component={Home}/>
    <Route path="/blog" component={Blog}/>
  </div>
</Router>
```

## match

Route组件默认会为其component组件提供match作为props参数，match对象包含以下几项属性：

* params 预设在url中传递的参数
* isExact 当前url是否绝对匹配此路由
* path 路由设定的path值
* url 当前的url

我们可以在路由设定的组件中使用match传递过来的数据，例如下面这个例子：

```jsx
import {
    BrowserRouter as Router,
    Route,
    NavLink as Link
} from 'react-router-dom'

<Router>
  <div>
    <ul>
        <li><Link activeStyle={{ fontWeight: 'bold' }} to="/repo/react">Repo:react</Link></li>
        <li><Link activeStyle={{ fontWeight: 'bold' }} to="/repo/vue">Repo:vue</Link></li>
        <li><Link activeStyle={{ fontWeight: 'bold' }} to="/repo/bootstrap">Repo:bootstrap</Link></li>
    </ul>

    <Route path="/repo/:repoName" component={Repo} />
  </div>
</Router>

const Repo = ({ match }) => (
    <div>
        <p>{match.params.repoName}</p>
    </div>
    )
```



## optional param

**React Router v1, v2 and v3**

在之前的版本当中，路由的可选参数通过下面代码这种形式进行设置。也就是用小括号把我们路由的可选部分括住。

```jsx
<Route path="to/page(/:pathParam)" component={MyPage} />
```

**React Router v4**

React Router v4 和之前的 v1-v3 版本有很大不同, 可选参数的写法也没有在文档中明确提出来，我们通过如下这种方式来传入路由可选参数，则需要在参数的结尾加上一个问号，表示它为可选的。如果不加上可选参数的符号，在我们没有传入参数时，以下面这个例子来说，它就不会渲染MyPage组件了。

```jsx
<Route path="/to/page/:pathParam?" component={MyPage} />
```

## withRouter

在react-router中，获取match对象的方法有许多种，除了Route组件的component属性中包含的组件可以获取match之外，我们还可以通过一个叫做withRouter的方法，直接将路由参数传递到我们的目标组件中，这一方法也就解决了类似之前介绍过的React当中context解决的问题。

withRouter也可以同redux一起协同使用：

```jsx
import { connect } from 'react-redux'
import { withRouter } from 'react-router'
import { TodoList } from '../Components/TodoList'
import { getVisibleTodos } from '../reducer'
import { toggleTodo } from '../action'

const mapStateToProps = (state, { match }) => ({
    todos: getVisibleTodos(
      state,
      match.params.filter || 'all'
      )
  })

export const TodoList = withRouter(connect()(TodoList))
```