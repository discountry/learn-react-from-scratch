## 2-1-JSX 简介

JSX 其是一个语法扩展，它既不是单纯的字符串，也不是 HTML，虽然长得和 HTML 很像甚至基本上看起来一样。但事实上它是 React 内部实现的一种，允许我们直接在 JS 里书写 UI 的方式。

有些同学来看 JSX 可能也会觉得它像一种模板语言之类的。事实上也不是，它就是基于 JavaScript，在 React 当中的一种语法扩展的实现。

JSX 被用来创建 React 当中的 Elements，React 当中的元素。然后 React 再通过一些方法，把 JSX 创建的元素，渲染成我们在浏览器当中看到的 DOM元素。

在正式介绍 JSX 的相关知识前，我再强调几遍：

* JSX 不是 HTML
* JSX 不是 HTML
* JSX 不是 HTML

JSX 尽管使用了非常多的类似 HTML 的标签语法，但 JSX 只是一种在 JavaScript 当中书写 UI 的实现，所以你在理解的时候不要把 HTML 的相关知识先入为主地代入。不要问为什么可以这样写，怎么能这么写，怎么和 HTML 不一样？一类的问题，因为本来就不是同一种东西，另外也不要觉得 JSX 奇怪，不要觉得不适应，你写写就适应了。而且相信我，JSX 已经是书写 UI 最高效还保持了良好可读性的一种实现了。

另外 JSX 也有一些和 HTML 语法的差异，本篇教程的内容不可能涵盖全部，但以后出现在教程代码中的特例我都会强调说明。

## JSX实现原理

我们想要在浏览器里直接运行采用 JSX 语法的 JavaScript 显然暂时是不可能实现的，在实际的生产过程中，我们需要利用 Babel 一类的转译器来将我们的 JSX 语法或者 ES6 语法转译成浏览器可以直接运行的 JavaScript，事实上 JSX 在经过转译之后，会变成 React 创建 Element 的一个方法：

```jsx
ReactDOM.render(
    <p>Hello world!</p>,
    document.getElementById('container')
)
```

转译之后就会变成下面这样：

```js
// 事实上你书写的所有标签语法最后都会被转换成创建元素的 JS 方法。
ReactDOM.render(
    React.createElement('p',null,`Hello world!`),
    document.getElementById('container')
)
```

## JSX 基本语法

在本地配置 React 的开发环境是相对来说比较复杂的操作。为了让同学们尽快上手，我们一开始还是在在线代码编辑器里编写我们的代码。

比如我们可以使用 React 官方推荐的 Codepen

打开 Codepen 网站，当然为了方便你日后的学习，保存你的代码，最好注册一个账号。（注册理所当然需要 Google 人机验证等步骤，所以请自备梯子，学习编程这方面的技能早晚要掌握，如果你怎么都打不开这个网站或者怎么都注册不好，可以先放过，老老实实使用大清局域网）。


之后我们新建一个 pen，然后在 JS 编辑窗口的设置面板里添加 react 和 react-dom 两个库，再将 preprocessor 预处理器设置成 Babel，确定保存，就可以开始愉快地编码啦！

* [Codepen 在线代码示例](https://codepen.io/discountry/pen/bBJamm?editors=1010)

当然，如果你的网速较慢或者无法顺利访问到国际网络，还可以使用离线Web应用 Codepan

* [使用 Codepan](https://codepan.net/boilerplate/react)


### JSX 元素

```jsx
const title = <h1>React Learning</h1>
```

我们用JSX创建的元素对象一般来说是不变的，所以通过 `const`关键字来声明一个 React元素，而不是我们以往经常使用的 `var`

为了能让我们的 JSX 元素在页面上渲染出来预览查看，我们还需要添加两段代码：

在 HTML 窗格里添加：

```html
<div id="root"></div>
```

在JS窗格的最底部添加：

```js
// 这便是将 JSX元素渲染成 DOM 的方法
ReactDOM.render(title,document.getElementById('root'))
```

我们通过 ReactDOM 的 render 方法，将 title 元素渲染至 id 为 root 的页面容器当中。

### JSX 属性

JSX 的标签同样可以拥有自己的属性：

```jsx
const title = <h1 id="main">React Learning</h1>
```

但它和 HTML 又不是完全相同的，例如我们想要为 JSX标签添加 class 的时候需要：

```jsx
// 注意是 className 而不是 class
const title = <h1 className="main">React Learning</h1>
```

所有支持的 HTML属性[在这里](https://facebook.github.io/react/docs/dom-elements.html#all-supported-html-attributes)可以查阅。

### JSX 嵌套

JSX 的标签也可以像 HTML 一样相互嵌套，一般有嵌套解构的 JSX 元素外面，我们习惯于为它加上一个小括号：

```jsx
const title = (
    <div>
        <h1 className="main">React Learning</h1>
        <p>Let's learn JSX</p>
    </div>
)
```

需要注意的是，JSX 在嵌套时，最外层有且只能有一个标签，否则就会出错袄：

```jsx
// 这是一个错误示例
const title = (            
    <h1 className="main">React Learning</h1>
    <p>Let's learn JSX</p>
)
```

### JSX表达式

在 JSX 元素中，我们同样可以使用 JavaScript 表达式，在 JSX 当中的表达式需要用一个大括号括起来：

```jsx
function sayhi(name) {
  return 'Hi,' + name
}

const title = (
    <div>
        <h1 className="main">React Learning</h1>
        <p>Let's learn JSX. <span>{sayhi('you')}</span></p>
    </div>
)
```

好了，有关 JSX 的基础知识，我们掌握到这里就差不多了。在之后的课程中，随着我们学习的进一步深入，也会在需要的时候介绍更深入的有关 JSX 的知识。